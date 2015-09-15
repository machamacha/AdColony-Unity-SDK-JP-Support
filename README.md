![AdColony Logo and Title](assets/logo-title.png)

##基本情報##

###AdColony SDK 取得URL###

https://github.com/AdColony/AdColony-Unity-SDK

***
AdColonyはアプリケーションのあらゆる場所にHD動画広告を配信することができます。動画を再生完了した時点でユーザに仮想通貨を付与するV4VC広告も提供しています。

##実装ガイド##
* Project Setup
  * [Unity and Xcode Project Setup](Unity-and-Xcode-Project-Setup.md)
  * [Unity Android Project Setup](Unity-Android-Project-Setup.md)
* [Showing Interstitial Videos](Showing-Interstitial-Videos.md)
* [Showing V4VC Videos](Showing-V4VC-Videos.md)
* [API Details](API-Details.md)
* [Troubleshooting,-F.A.Q.,-and-Sample-Applications](Troubleshooting,-F.A.Q.,-and-Sample-Applications.md)
* [よくある質問](QA.md)

###Note for Unity 5:###

Unity inspectorから`Assets/Plugins/iOS/UnityADC`内の`Platform Settings`を見つけて、`Compile flags`にフラッグ`-fno-objc-arc`を追加すれば、xcodeプロジェクトビルと時の問題解決することができます。
