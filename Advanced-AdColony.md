##AdColony Methods##

The following AdColony static methods are available.  `AdColony.Configure()` must be called before any of the others.  Assign event delegates (listed on the next page) before calling `AdColony.Configure()`.

`Configure(  app_version:string, app_id:string, zone_id_1:string, ... )`
  * Configures AdColony with one or more Video Zone IDs.

`SetCustomID( custom_id:string )`
  * Once you have provided an identifier, AdColony will persist it across app restarts (stored on disk only) until you update it. If using this method, call it before `Configure` so that the identifier will be used consistently across all server communications. The identifier will also pass through to server-side V4VC callbacks.

`GetCustomID() : string`
  * Returns the ID previously set by calling `SetCustomID`

`GetDeviceID() : string`
  * Returns the MAC Address of the device.

`GetOpenUDID() : string`
  * Returns a device identifier as reported by the OpenUDID library.

`GetODIN1() : string`
  * On iOS -- Returns a device identifier as specified by the ODIN1 standard.
  * On Android -- This will always return the string **undefined**.

`GetV4VCAmount() : int`
  * Returns the amount that can be obtained from `ShowV4VC()`.

`GetV4VCName() : string`
  * Returns the name of the virtual currency as set on the AdColony server.

`IsV4VCAvailable() : bool`
  * The player will get a virtual currency reward after the ad.

`isVideoAvailable() : bool`
  * Returns "true" if a video will play when you call `ShowVideoAd()`.

`OfferV4VC( postPopup:bool )`

`OfferV4VC( postPopup:bool, zone_id:string )`
  * Shows a built-in popup asking the user if they want to watch a video for virtual currency.  If they say "yes" then the video will automatically be shown.  If no *zone_id* is given than the default is used.

`ShowV4VC( postPopup:bool )`

`ShowVideoAd( postPopup:bool, zone : string )`
  * Attempts to play a video for virtual currency. If no *zone_id* is given than the default is used.

`ShowVideoAd()`

`ShowVideoAd( zone : string )`
  * Attempts to play a video ad. If no *zone_id *is given than the default is used.AdColony Event Delegates

`notifyIAPComplete(product_id:string, trans_id:string, currency_code:string, price:double )`
`notifyIAPComplete(product_id:string, trans_id:string, currency_code:string, price:double, quantity:int )`
  * Notify after an IAP Transaction has been completed. For use with IAP Promo ads.
  * The iOS version of this function takes an int for the quantity, this is defaulted to 1.
  * The Android version of this function does not take a quantity.

`OnVideoFinished()`
  * This delegate is called both when a video finishes playback and when no video plays after a call to `ShowVideo()` or `ShowV4VC()`.

`OnV4VCResult( bool success, string name, int amount )`
  * This delegate is called when a V4VC video has played and the virtual currency result is available.

`VideoFinishedWithInfoDelegate( AdColonyAd ad_shown )`
  * This delegate is called both when a video finished playback and when no video plays after a call to `ShowVideo()` or `ShowV4VC()`, and includes information about the ad if it played. 


##Managing Application Status##

In the AdColony Publisher tab of the AdColony Portal, you will notice the application status when you create new applications:

[[assets/adr/image_5.png|alt=AdColony Application status]]

The following is a detailed explanation of each status:

| STATUS | DESCRIPTION |
| ------ | ----------- |
| Testing | Indicates that one or more video zones are showing "Test Ads". To remove the “Testing” status from a zone, click on the video zone and select “No” in the Development (show test ads only) section. |
| Active | As soon as the developer integrates the SDK, sets up their zone and select “No” in the Development section,  the system will auto switch the application status to "Active".Inactive Indicates that no video zones are created or all video zones have been inactivated.  To inactivate video zones, go inside the video zone and select “No” to Integration (zone is active) section.|

**Note:** Even if your application status is "Testing", you can still receive live video ads to your application provided that you have at least one video zone in "Active" status.  We recommend that you deactivate video zones you are not using so that the proper application status is displayed.

##Test Ads, Live Ads, and Switching##

AdColony provides test ads that perform identically to live ads, with few exceptions. Test ads will not affect your account balance. In V4VC zones, test ads will not obey the ‘Daily Max Per User’ setting. We have a policy to avoid directing live ads to applications that are still in development or testing.

New video zones automatically default to receive test ads.  Please note that if you set your zone to receive live ads before your application is live on a user-facing marketplace with AdColony included, it may not receive any ads.

You can toggle test ads in the Publisher section of the[ clients.adcolony.com](http://clients.adcolony.com/) control panel.

1. Ensure your App’s ‘SDK integration’ indicates communication with the AdColony server (please note this status updates roughly hourly).

2. Publishers->Click on your app’s link-> **Edit.**

3. Click on the link for the **Video Zone** in which you’d like to change the type of video ads (Test or Live Video Ads).

4. Select the corresponding radio button in the **Show test ads only (for dev or debug)?** section and click **Save**.  Do this for each video zone in your app

* * *
