##よくある質問##
Contents

*  [基本情報に関して](#基本情報に関して)
*  [SDK仕様に関して](#sdk仕様に関して)
*  [動画再生に関して](#動画再生に関して)
*  [ストア申請に関して](#ストア申請に関して)
    

###基本情報に関して###
- Q:各設定情報はどんな意味ですか
- A:
	- **App ID**: こちらは各アプリを指します。
	- **Zone ID**: こちらはアプリの下に紐づく、各掲載枠を指します。
	- **Call Back URL**: 動画再生の成果等を送るURLになります。設定していただかなくても本サービスはご利用頂けます。
	- **Custom ID**: CustomIDは、mediaのユーザーidを設定していただきます。設定した値はcall backを使用する場合、custom_idとして返します。
	- **UDID**: 端末に紐づく固有のIDです。
		*UDIDはAppleで取得を禁じられているため、AdColonyでは取得しておりません。*

###SDK仕様に関して###
- Q: 縦画面の再生は可能か？
- A: AdColonyでは、スキップなし横画面フルサイズの再生となります。

- Q: AdColonyダイアログででポップアップの国別で表記を可能か？
- A: アプリの配信国に沿った各々の表記を行う場合、アプリ内でダイアログのご実装をご自身でして頂く必要がございます。

- Q: 上限回数は何回ですか？
- A: 上限回数は変更可能です。変更希望の場合、担当者もしくは(video-ad@glossom.co.jp)までご連絡下さい。

- Q: インセンティブはコイン以外でも可能か？
- A: 可能です。御社側で、動画視聴の成果通知に合わせてご実装して頂く必要がございます。

- Q: custom_idの設定する方法おしえてください。
- A: CustomIDの設定はSDK側でconfigure関数の前に、下記のように設定してください。

            AdColony.SetCustomID("xxxx");
            https://github.com/AdColony/AdColony-Unity-SDK/wiki/API-Details#setcustomid


- Q:  V4VCのサーバー側のレスポンスと再送信
- A: 下記でございます。

		- "vc_success"
		正常に処理が終わり、ユーザーへのコイン付与が成功した場合、AdColony側から再送信を行いません。
	
		- "vc_decline" または "vc_noreward"
		正常に処理が終わったが、uidの誤り/不正と判断された場合、AdColony側から再送信を行いません。
	
		- [上記以外に、他にレスポンスされる場合]
		AdColonyは定期的に再送信行います。異常な場合以外は、こちら利用は控えて下さい。

###動画再生に関して###
- Q: 動画再生ができない場合どうすればいいですか
- A: 下記をチェックしてください。
	- 正しいIDは使われているか？
		- 発行されたApp ID/Zone IDの対象OSをお誤りのないよう、ご利用下さい。
	- IDと実装マニュアルの形式は同様か？
		- 動画リワードの場合[Showing V4VC Videos]
		- interstisialの場合[Showing Interstitial Videos]をご利用下さい。
	- 広告がダウンロードされていない
		- 再生を行うにあたって、動画をダウンロードする時間が必要です。
		- 広告準備が確認された後、再生を行って下さい。
		- 広告の再生可否をの検出方法を参考してください。

- Q: 広告の再生可否を検出するためには？
- A: 下記のリスナーを実装して頂くと、動画再生可能な状態になると通知を受けることが可能になります。再生準備が終わるまで、再生ボタンを非表示する処理を追加で行って頂くことを推奨致します。

            public delegate void AdAvailabilityChangeDelegate( bool available, string zoneId );
            https://github.com/AdColony/AdColony-Unity-SDK/wiki/API-Details#onadavailabilitychange

- Q: 配信可能な広告がない（少ない）ですが、どうすればいいですか
- A: 広告在庫は様々なロジックで配信制限を行っております。詳細は下記を参考にして下さい。
	- ユーザーが不正と判断された場合
	- 1ユーザあたりの配信上限回数への到達
	- eCPMが極端に低い
	- ユーザー数が極端に少ない、リリース前もしくはリリース直後
	- androidの場合、GoogleのAdvertising IDを取得するため、プロジェクトの中にGoogle Play Services 4.0+ を追加してください。

###ストア申請に関して###
- Q: App内で広告を消した方が良いか(iOS)？
- A: 審査する場合も、広告を出してください。IDFAについては下記をチェックを必ず行って下さい。
	- Does this app use the Advertising Identifier (IDFA)?
		- Yes
	- Serve advertisements within the app
		- チェック
- Q: テスト切り替えの際の連絡はいつしたら良いか？
- A: ストア通過後(iOS)に、またはリリース前(Android)に担当者へ配信切り替えのご連絡をして下さい。
