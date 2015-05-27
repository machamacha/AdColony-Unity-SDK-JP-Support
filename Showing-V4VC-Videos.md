###概要###
AdColony V4VC (Videos-for-Virtual-Currency)はAdColony Interstitial広告の上で実装した動画広告が再生完了した時点で、ユーザーに仮想通貨を付与することができるシステムです。  
AdColony V4VCはユーザーの仮想通貨残高は追跡しません。ユーザーに仮想通貨を付与すべき時点でアプリケーションに通知する機能を提供しています。

クライアント側で仮想通貨の残高を管理してるアプリには、V4VCのクライアントモードで実装することが出来ます。   サーバー側で仮想通貨の残高を管理している場合、よりセキュリティーの高いサーバー側モードによって、付与する時点でサーバーに通知することも可能です。

###Client-side Mode Instructions###
先に[Unity and Xcode Project Setup](Unity-and-Xcode-Project-Setup.md)(iOS), または  [Unity Android Project Setup](Unity-Android-Project-Setup.md)(Android) を実装してください。 クライアントモードは[Showing Interstitial Videos](Showing-Interstitial-Videos.md)の実装方法とほぼ同じです。

1. Glossomにてapp ID、zone IDの発行しお渡し致します。
2. Glossom側にて枠の"Virtual Currency Rewards" を有効し、仮想通貨名（currency name） と 仮想通貨額（reward amount） を設定します。
4. `AdColony.OnV4VCResult` にリワードコールバックを実装してください。その中でユーザーの仮想通貨の残高とそのUIを更新してください。
5. AdColonyのV4VCの動画広告とポップアップメッセージを表示するコードを実装してください。

###Code Example###
下記はAdColony Unity pluginと一緒に配布されたサンプルです。クライアントモードでのV4VC広告の実装を説明しています。


**MyV4VC.m**
```csharp
public class MyV4VC : MonoBehaviour
{
  public void Initialize()
  {
    // Assign any AdColony Delegates before calling Configure
    AdColony.OnVideoFinished = this.OnVideoFinished;
    AdColony.OnV4VCResult = this.OnV4VCResult;

    // If you wish to use a the customID feature, you should call  that now.
    // Then, configure AdColony:
    AdColony.Configure
    (
      "version:1.0,store:google", // Arbitrary app version and Android app store declaration.
      "appbdee68ae27024084bb334a",   // ADC App ID from adcolony.com
      "vzf8fb4670a60e4a139d01b5", // A zone ID from adcolony.com
      "vzf8fb4670a60e4a139d01b5", // Any number of additional Zone IDS
      "vz1fd5a8b2bf6841a0a4b826"
    );
  }

  // When a video is available, you may choose to play it in any fashion you like.
  // Generally you will play them automatically during breaks in your game,
  // or in response to a user action like clicking a button.
  // Below is a method that could be called, or attached to a GUI action.
  public void PlayV4VCAd( string zoneID, bool prePopup, bool postPopup )
  {
    // Check to see if a video for V4VC is available in the zone.
    if(AdColony.IsV4VCAvailable(zoneID))
    {
      Debug.Log("Play AdColony V4VC Ad");
      // The AdColony class exposes two methods for showing V4VC Ads.
      // ---------------------------------------
      // The first `ShowV4VC`, plays a V4VC Ad and, optionally, displays
      // a popup when the video is finished.
      // ---------------------------------------
      // The second is `OfferV4VC`, which popups a confirmation before
      // playing the ad and, optionally, displays popup when the video
      // is finished.

      // Call one of the V4VC Video methods:
      // Note that you should also pause your game here (audio, etc.) AdColony will not
      // pause your app for you.
      if (prePopup) AdColony.OfferV4VC( postPopup, zoneID );
      else          AdColony.ShowV4VC( postPopup, zoneID );
    }
    else
    {
      Debug.Log("V4VC Ad Not Available");
    }
  }

  private void OnVideoFinished(bool ad_was_shown)
  {
    Debug.Log("On Video Finished");
    // Resume your app here.
  }

  // The V4VCResult Delegate assigned in Initialize -- AdColony calls this after confirming V4VC transactions with your server
  // success - true: transaction completed, virtual currency awarded by your server - false: transaction failed, no virtual currency awarded
  // name - The name of your virtual currency, defined in your AdColony account
  // amount - The amount of virtual currency awarded for watching the video, defined in your AdColony account
  private void OnV4VCResult(bool success, string name, int amount)
  {
    if(success)
    {
      Debug.Log("V4VC SUCCESS: name = " + name + ", amount = " + amount);
      this.currency += amount;
    }
    else
    {
      Debug.LogWarning("V4VC FAILED!");
    }
  }
}
```
上記の通り、`Configure`を使って設定します。
interstitialでの導入方法と大きな違いはございませんが、`Configure`実行前に、`OnV4VCReward`プロパティーにコールバックを設定しています。

そのコールバックは動画再生が終わった後に呼び出され、その中でユーザーに仮装通貨を付与することができます。このコードはアプリ起動をする場所へ記述して下さい。通常は MonoBehaviour の`Initialize`メソッドになります。
その際、AdColony app ID と zone IDsをGlossomで発行されたご自身のIDに入れ替える必要があります。

上記ではAdColonyから`success`パラメータをコールバックに渡してます。クライアントサイドV4VCでは、このパラメーターがAdColonyのサーバーからこのリワードを認証したかどうかを意味しています。

成功した場合、このメソッドではリワードアマウントをユーザーの通貨情報に追加し、さらにアプリのUIに対しても情報を更新するよう設定してください。失敗した場合、不正の対象の可能性になるため、ユーザーに通貨（対価）を付与しないようにしてください。安全のため、このサンプルでは、その後V4VC広告をアプリ内で表示しないように設定してあります。

AdColonyには動画再生メソッドが二つあります：`ShowV4VC` と `OfferV4VC`、どちらも動画再生が終わった後にユーザーに確認のポップアップを出すことが可能です。しかし、`OfferV4VC`は動画再生前にもポップアップを出すことになります。この二つのメソッドはアプリのどこでもコールし動画を再生するが可能です。その際、AdColony app ID と zone IDsをGlossomで発行されたご自身のIDに入れ替える必要があります。

下記は、AdColonyの動画再生をする前と後、両方のポップアップ例です。
![AdColony V4VC Pre-Popup](assets/pre-popup.jpg)
![AdColony V4VC Post-Popup](assets/post-popup.jpg)

--

###Server-side Mode Instructions###
サーバ側モードは、AdColonyがアプリに仮想通貨を付与する通知を行う前に、アプリのサーバー側に先に通知する以外は、クライアントモードと実装方法はほぼ同様のものとなります。  AdColonyはアプリのサーバーにユーザーとリワードを含めて様々な情報を通知致します。それをもってサーバー側がその成果を認証または否認証することができます。この方式を実装するにはクライアントモード手順を実装した上で、下記を実装してください。

1. `AdColony.OnV4VCResult` にリワードコールバックを実装してアプリサーバーからもらった仮想通貨の情報を更新してください。
2. AdColony SDKにユーザ識別子(custom identifier)を設定してください。これは毎回リワードコールバックするときにアプリのサーバー側にも送信されます。これを使用することで、通貨を付与するユーザを特定することができます。
3. Glossomにてzoneの設定を有効にし、ポストバック用のURLを設定致します。
4. ポストバック先のURL実装、そして有効なレスポンスをしてください。

###Code Example - Server-side Callback###
server-side V4VCを設定したzoneを利用する場合、ポストバック先のURLは有効なレスポンスをするため、下記の手順に従って確認をしてください。

1. 発行されたzoneの秘密鍵を利用してURLパラメータをチェックし、正しいポストバックURLかどうかを確認してください。
2. パラメーターに含まれたユーザーIDが有効であるかどうかを確認してください。
3. 重複チェックを行って、ユーザーに指定したアマウントとタイプの通貨を付与してください。
4. 上記を踏まえて、有効なレスポンスをリターンしてください。

AdColonyからコールバックするURLの仕様は下記でございます。括弧内に囲まれているパラメータはアプリケーション実装時の情報により動的にセットされます。

```
[http://www.example.com/anypath/callback_url.php]?id=[transaction id]&uid=[AdColony device id]&amount=[currency amount to award]&currency=[name of currency to award]&open_udid=[OpenUDID]&udid=[UDID]&odin1=[ODIN1]&mac_sha1=[SHA-1 of MAC address]&verifier=[security value]
```

URL Parameter | Type | Purpose
--- | --- | ---
id | Positive long integer | Unique V4VC transaction ID
uid | String | AdColony device ID
amount | Positive integer | Amount of currency to reward
currency | String | Name of currency to reward
open_udid | String | OpenUDID
udid | String | Apple UDID
odin1 | String | Open Device Identification Number (ODIN)
mac_sha1 | String | SHA-1 hash of lowercase colon-separated MAC address
custom_id | String | Custom user ID (if set client-side)
verifier | String | MD5 hash for message security

上記の情報でユーザーを識別できない場合、下記のように、ユーザーを識別するために、カスタマイズユーザーIDを設定してください。よく使われるのはvendor identifier(アプリ内でのユニークなID）がございます。

1. AdColonyのアカウント情報を設定する*前*に、`AdColony.SetCustomID`を呼び出してユーザーIDを設定してください。このIDはサーバーとのすべてのコミュニケーションに使用されます。
2. Zoneの設定画面でコールバックURLの最後に `&custom_id=[CUSTOM_ID]` を追加してください。


参考例：下記はPHP + MySQLで実装した例です。

```php
<?php
	$MY_SECRET_KEY = "This is provided by adcolony.com and differs for each zone";

	$trans_id = mysql_real_escape_string($_GET['id']);
	$dev_id = mysql_real_escape_string($_GET['uid']);
	$amt = mysql_real_escape_string($_GET['amount']);
	$currency = mysql_real_escape_string($_GET['currency']);
	$open_udid = mysql_real_escape_string($_GET['open_udid']);
	$udid = mysql_real_escape_string($_GET['udid']);
	$odin1 = mysql_real_escape_string($_GET['odin1']);
	$mac_sha1 = mysql_real_escape_string($_GET['mac_sha1']);
	$custom_id = mysql_real_escape_string($_GET['custom_id']);
	$verifier = mysql_real_escape_string($_GET['verifier']);

	$success_string = "vc_success";
	$fail_string = "vc_noreward";
	// IMPORTANT: for Android SDK less than version 2.1.0, you must use "vc_decline", i.e.:
	// $fail_string = "vc_decline"

	//verify hash
	$test_string = "" . $trans_id . $dev_id . $amt . $currency . $MY_SECRET_KEY .
		$open_udid . $udid . $odin1 . $mac_sha1 . $custom_id;
	$test_result = md5($test_string);
	if($test_result != $verifier) {
		echo $fail_string;
		die;
	}

	$user_id = //TODO: get your internal user id using one of the supplied identifiers
	// OpenUDID, AdColony ID, ODIN1, custom ID can be accessed via a method call in the AdColony client SDK

	//check for a valid user
	if(!$user_id) {
		echo $fail_string;
		die;
	}
	//insert the new transaction
	$query = "INSERT INTO AdColony_Transactions(id, amount, name, user_id, time) ".
		"VALUES ($trans_id, $amt, '$currency', $user_id, UTC_TIMESTAMP())";
	$result = mysql_query($query);
	if(!$result) {
		//check for duplicate on insertion
		if(mysql_errno() == 1062) {
			echo $fail_string;
			die;
		}
		//otherwise insert failed and AdColony should retry later
		else {
			echo "mysql error number".mysql_errno();
			die;
		}
	}
	//TODO: award the user the appropriate amount and type of currency here
	echo $success_string;
?>
```
注意、`TODO`のところにはアプリケーション特定のロジックを実装してください。上記のコードで使ったMySQLデータベースは下記のSQLで作成できます。(通貨名を`enum`にインサートするのを忘れないようにして下さい。

```mysql
CREATE TABLE `AdColony_Transactions` (
  `id` bigint(20) NOT NULL default '0',
  `amount` int(11) default NULL,
  `name` enum('Currency Name 1') default NULL,
  `user_id` int(11) default NULL,
  `time` timestamp NULL default NULL,
  PRIMARY KEY  (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```
