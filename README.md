![AdColony Logo and Title](assets/logo-title.png)

##基本情報##

###AdColony SDK 取得URL###

https://github.com/AdColony/AdColony-Unity-SDK

***
AdColonyはアプリケーションのあらゆる場所にHD動画広告を配信することができます。動画を再生完了した時点でユーザに仮想通貨を付与するV4VC広告も提供しています。

###iOS9###
iOS9にて追加された新しい仕様の中に、本SDKの実装に影響を及ぼすものが存在します。  
アプリをiOS9ターゲット(Xcode 7以降)でビルドするためには、  
[iOS9実装手順](https://github.com/glossom-dev/AdColony-iOS-SDK-JP-Support/blob/master/iOS-9.md)に記載された手順に従った実装が必要となります。  

##実装ガイド##
* Project Setup
  * [Unity and Xcode Project Setup](Unity-and-Xcode-Project-Setup.md)
  * [Unity Android Project Setup](Unity-Android-Project-Setup.md)
* [Showing V4VC Videos](Showing-V4VC-Videos.md)
* [Showing Interstitial Videos](Showing-Interstitial-Videos.md)
* [API Details](API-Details.md)
* [Troubleshooting,-F.A.Q.,-and-Sample-Applications](Troubleshooting,-F.A.Q.,-and-Sample-Applications.md)
* [よくある質問](QA.md)

###Note for Unity 5:###

Unity inspectorから`Assets/Plugins/iOS/UnityADC`内の`Platform Settings`を見つけて、`Compile flags`にフラッグ`-fno-objc-arc`を追加すれば、xcodeプロジェクトビルと時の問題解決することができます。
