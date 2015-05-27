AdColonyのinterstitial広告は動画に続いてエンドカードが表示される動画広告です。

###Instructions###
[Unity and Xcode Project Setup](Unity-and-Xcode-Project-Setup.md)(iOS) と [Unity Android Project Setup](Unity-Android-Project-Setup.md)(Android) を実装した後に、下記の手順でinterstitial広告を表示するすることができます：

1. Glossomにてapp ID、zone IDを発行しお渡し致します。
2. アプリが起動したところにAdColonyの設定関数（configure）でapp ID, zone IDを設定してください。
3. 広告を表示するコードを実装してください。

###Code Example - AdColonyBasic Sample App###
SDKと同梱されているサンプルアプリはinterstitial広告を表示させる手順を記載しております。
下記のサンプルでは簡単にMonoBehaviourでAdColony広告を表示させる手順を示しています。
詳しい利用方法はSDKと同梱されているサンプルアプリをご参考ください。

ほとんどの場合、アプリのエントリーポイントはMonoBehaviouクラスのメソッド`Initialize`、そこでAdColonyの設定関数`Configure` を実行すると、AdColonyがアプリ起動直後にすぐ動画広告の配信準備に着手することができます。  注意：サンプルの中のapp idとzone idを自分のIDに変更するのを忘れないようにしてください。

**MyMain.cs**
```csharp
public class MyMain : MonoBehaviour
{
  public void Initialize()
  {
    // Assign any AdColony Delegates before calling Configure
    AdColony.OnVideoFinished = this.OnVideoFinished;

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
  public void PlayAVideo( string zoneID )
  {
    // Check to see if a video is available in the zone.
    if(AdColony.IsVideoAvailable(zoneID))
    {
      Debug.Log("Play AdColony Video");
      // Call AdColony.ShowVideoAd with that zone to play an interstitial video.
      // Note that you should also pause your game here (audio, etc.) AdColony will not
      // pause your app for you.
      AdColony.ShowVideoAd(zoneID);
    }
    else
    {
      Debug.Log("Video Not Available");
    }
  }

  private void OnVideoFinished(bool ad_was_shown)
  {
    Debug.Log("On Video Finished");
    // Resume your app here.
  }
}
```
上記のように`ShowVideoAd`メソッドは指定したzoneの動画をすぐに再生できます。
このメソッドはアプリのどこからでも実行ができます。例えば、あるGUIボタンを押した後に`PlayAVideo`を実行することも出来ます。
zoneIDをご自身のzone idに設定するようにしてください。

上記の例では、`OnVideoFinished`delegateメソッドの設定もしました。
このメソッドは動画が再生し終わった直後に呼び出されますので、この時点で、動画再生するために止まったゲームの再開処理をすることが出来ます。

### 注意点 ###
1. AdColonyの動画再生メソッドが呼ばれた時点で、動画が準備完了してない可能性があります。
その場合、動画メソッドを呼び出しても特に何も表示されません。configuringした後に少し時間が経ってから再生メソッドを実行してください。
2. 音楽が絶えず流れるアプリ側の場合、動画再生中は音楽やその他の音を全て止めてください。
`AdColony`からのコールバックメソッドを利用して、動画再生開始するときと完了するときにアプリ側の音楽を一時停止/再開することができます。
`AdColony` delegatesメソッドについては、[ここ（API詳細）](API-Details.md#onvideostarted)をご参照頂くか、サンプルアプリに参考にしてください。
