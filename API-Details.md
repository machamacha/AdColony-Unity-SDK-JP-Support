[AdColony Class Reference](API-Details#adcolony-class-reference)

===
###AdColony Class Reference###
####Configuring AdColony####
[`Configure( string appVersion, string appID, string zoneID_1, ... )`](API-Details#configure)  
####Playing Ads####
[`OfferV4VC( bool postPopup, [string zoneID] )`](API-Details#offerv4vc )  
[`ShowV4VC( bool postPopup, [string zoneID] )`](API-Details#showv4vc)  
[`ShowVideoAd( [string zoneID] )`](API-Details#showvideoad)  
####Ad Availability####
[`IsVideoAvailable( [string zoneID] `](API-Details#isvideoavailable)  
[`StatusForZone( [string zoneID] `](API-Details#statusforzone)  
####Device and User Identifiers####
[`SetCustomID( string customID ) `](API-Details#setcustomid)  
[`GetCustomID`](API-Details#getcustomid)  
[`GetOpenUDID`](API-Details#getopenudid)  
[`GetDeviceID`](API-Details#getuniquedeviceid)  
[`GetODIN1`](API-Details#getodin1)
####V4VC - Availability and Rewards####
[`IsV4VCAvailable( [string zoneID] )`](API-Details#isv4vcavailable)  
[`GetV4VCAmount( [string zoneID] )`](API-Details#getv4vcamount)  
[`GetV4VCName( [string zoneID] )`](API-Details#getv4vcname)  
[`notifyIAPComplete( string product_id, string trans_id, [string currency_code], [double price], [int quantity]  )`](API-Details#notifyiapcomplete)  
####Delegate Methods####
[`OnVideoStarted`](API-Details#onvideostarted)  
[`OnVideoFinished( bool ad_shown )`](API-Details#onvideofinished)  
[`OnVideoFinishedWithInfo( AdColonyAd info)`](API-Details#onvideofinishedwithinfo)  
[`OnV4VCResult( bool success, string currencyName, int amount )`](API-Details#onvideofinished)  
[`OnAdAvailabilityChange( bool available, string zone_id )`](API-Details#onadavailabilitychange)  
####Types####
[`IAPEngagementType`](API-Details#iapengagementtype)  
[`AdColonyAd`](API-Details#adcolonyad)  


###Class Methods###

####Configure####
Configures AdColony specifically for your app; required for usage of the rest of the API.
```cs
static public void Configure( string appVersion, string appID, string zoneID_1, ... )
```
**Parameters**
* *appVersion*
  * **Example:** "version:1.0,store:google"
  * **Android** uses this parameter to define the store as well as assigning an arbitrary version number. Please note that if you are integrating into an Amazon app you will need to replace 'google' with 'amazon' in the appVersion String.
  * **iOS** only uses the version number. It will ignore any Android store declarations.
* *appID*
  * The AdColony app ID for your app. This can be created and retrieved at the [Control Panel](http://clients.adcolony.com).
* *zoneIDs*
  * An array of strings containing at least one AdColony zone ID string. AdColony zone IDs can be created and retrieved at the [Control Panel](http://clients.adcolony.com). If `null`, app will be unable to play ads and AdColony will only provide limited reporting and install tracking functionality.

**Discussion**
This method returns immediately; any long-running work such as network connections are performed in the background. AdColony does not begin preparing ads for display or perform any reporting until after it is configured by your app.

---
####OfferV4VC####
Prompts the user to play an AdColony V4VC video ad. If the user accepts, an AdColony V4VC ad is played.
```cs
static public void OfferV4VC( bool postPopup, [string zoneID] )
```
**Parameters**
* *postPopup*
  * Whether or not to show a confirmation popup upon finishing the ad view.
* *zoneID* [OPTIONAL]
  * The zone from which the ad should play.

**Discussion**
This method returns immediately, before the ad completes. If ads are not available, an ad may not play as a result of this method call. If you need more detailed information about when the ad completes be sure to define the `OnVideoFinished` callback. If postPopup is `true`, a confirmation dialog will be displayed to the user.

---
####ShowV4VC####
Plays an AdColony V4VC ad if one is available.
```cs
static public void ShowV4VC( bool postPopup, [string zoneID] )
```
**Parameters**
* *postPopup*
  * Whether or not to show a confirmation popup upon finishing the ad view.
* *zoneID* [OPTIONAL]
  * The zone from which the ad should play.

**Discussion**
This method returns immediately, before the ad completes. If ads are not available, an ad may not play as a result of this method call. If you need more detailed information about when the ad completes be sure to define the `OnVideoFinished` callback. If postPopup is `true`, a confirmation dialog will be displayed to the user.

---
####ShowVideoAd####
Plays an AdColony ad.
```cs
static public void ShowVideoAd( [string zoneID] )
```
**Parameters**
* *zoneID* [OPTIONAL]
  * The zone from which the ad should play.

**Discussion**
This method returns immediately, before the ad completes. If ads are not available, an ad may not play as a result of this method call. If you need more detailed information about when the ad completes make sure assign a callback to `OnVideoFinished`.

---
####IsVideoAvailable####
Returns whether or not a video is available in the specified zone.
```cs
static public bool IsVideoAvailable( string zoneID )
```
**Parameters**
* *zoneID* [OPTIONAL]
  * The zone from which the ad should play.

**Return Value**
Returns `true` if a video is available or `false` if no video is available.

####StatusForZone####
Returns status of the AdColony zone passed in.
```cs
static public string StatusForZone( string zoneID )
```
**Parameters**
* *zoneID* [OPTIONAL]
  * The zone from which the ad should play.

**Return Value**
Returns one of the following strings:
  * "invalid" - You've entered an incorrect zone id, or AdColony has not been configured with that zone ID.
  * "unknown" - Indicates that AdColony has not yet received the zone's configuration from the server.
  * "off" - The zone is disabled. The zone has been turned off on the control panel.
  * "loading" - Indicates that the zone is preparing ads for display.
  * "active" - The zone is enabled and has ads ready to be played.

---
####setCustomID:####
Assigns your own custom identifier to the current app user.
```cs
static public void setCustomID( string customID )
```
**Parameters**
* *customID*
  * An arbitrary, application-specific identifier string for the current user. Must be less than 128 characters.

**Discussion**
Once you've provided an identifier, AdColony will persist it across app restarts (stored on disk only) until you update it. If using this method, call it before [`Configure`](API-Details#configure) so that the identifier is used consistently across all server communications. The identifier will also pass through to server-side V4VC callbacks.

---
####getCustomID####
Returns the device's current custom identifier.
```cs
static public string getCustomID()
```
**Return Value**
The custom identifier string most recently set using [`setCustomID`](API-Details#setcustomid).

---
####getOpenUDID####
Returns the device's OpenUDID.
```cs
static public string getOpenUDID()
```
**Return Value**
The string representation of the device's OpenUDID.

**Discussion**
OpenUDID is a community-designed replacement for the Apple UDID. You can link your own copy of the OpenUDID library if desired, and it should return the same value for the OpenUDID. For details, please see the [OpenUDID GitHub page](https://github.com/ylechelle/OpenUDID).

As of iOS 7, the behavior of this identifier will change. We do not recommend using this identifier for new integrations. This method is provided for backwards compatibility.

---
####GetDeviceID####
Returns an AdColony-defined device identifier.
```cs
static public string GetDeviceID()
```
**Return Value**
The string representation of the device's AdColony identifier.

**Discussion**
The identifier is a SHA-1 hash of the lowercase colon-separated MAC address of the device's WiFi interface.

As of iOS 7, the behavior of this identifier will change. We do not recommend using this identifier for new integrations. This method is provided for backwards compatibility.

---
####getODIN1####
Returns the device's ODIN-1
```cs
static public string getODIN1
```
**Return Value**
The string representation of the device's ODIN-1.

**Discussion**
ODIN-1 is a community-designed replacement for the Apple UDID. You can link your own copy of the ODIN-1 source if desired, and it should return the same value. For details, please see the [ODIN-1 Google Code page](https://code.google.com/p/odinmobile/wiki/ODIN1).

As of iOS 7, the behavior of this identifier will change. We do not recommend using this identifier for new integrations. This method is provided for backwards compatibility.

**Not Supported in Android.** -- This will always return the string `undefined`.

---
####IsV4VCAvailable####
Returns whether or not it is possible for the user to receive a virtual currency reward for playing an ad in the zone.
```cs
static public bool isV4VCAvailable( [string zoneID] )
```
**Parameters**
* *zoneID* [OPTIONAL]
  * The zone in question.

**Return Value**
A boolean indicating whether a reward is currently available for the user.

**Discussion**
This method takes into account whether V4VC has been configured properly for the zone on the [AdColony Control Panel](http://clients.adcolony.com), whether the user's daily reward cap has been reached, and whether there are ads available.

---
####GetV4VCName:####
Returns the name of the virtual currency rewarded by the zone.
```cs
static public string GetV4VCName( [string zoneID] )
```
**Parameters**
* *zoneID* [OPTIONAL]
  * The zone in question.

**Return Value**
A string name of the virtual currency rewarded by the zone, as configured on the [AdColony Control Panel](http://clients.adcolony.com).

**Discussion**
You must first configure AdColony using [`Configure`](API-Details#configure).

---
####GetV4VCAmount:####
Returns the amount of virtual currency rewarded by the zone.
```cs
static public int GetV4VCAmount( [string zoneID] )
```
**Parameters**
* *zoneID* [OPTIONAL]
  * The zone in question.

**Return Value**
An integer indicating the amount of virtual currency rewarded by the zone, as configured on the [AdColony Control Panel](http://clients.adcolony.com).

**Discussion**
You must first configure AdColony using [`Configure`](API-Details#configure).

---
####notifyIAPComplete:####
Notify the SDK for an IAP after an IAP Promo Ad.
```cs
static public void notifyIAPComplete( string product_id, string trans_id, [string currency_code], [double price], [int quantity]  )
```
**Parameters**
 * product_id
  * The Product ID of the IAP
 * trans_id
  * The Transaction ID for the IAP
 * currency_code [OPTIONAL]
  * Currency code for the IAP
 * price [OPTIONAL]
  * Price of the purchase
 * quantity [OPTIONAL/iOS Only]
  * Amount of the purchase


---
####OnVideoStarted####
Your class can define a method matching this delegate signature and assign it to the corresponding property. Notifies your app when an ad begins displaying.

```cs
public delegate void VideoStartedDelegate();
//Delegate Property
public static VideoStartedDelegate OnVideoStarted;
```

---
####OnVideoFinished####
Your class can define a method matching this delegate signature and assign it to the corresponding property.
Notifies your app after an ad is displayed.

```cs
public delegate void VideoStartedDelegate( bool ad_shown );
//Delegate Property
public static VideoFinishedDelegate OnVideoFinished;
```

---
####OnVideoFinishedWithInfo####
Your class can define a method matching this delegate signature and assign it to the corresponding property.
Notifies your app after an ad is displayed, with information about that ad.

```cs
public delegate void VideoStartedDelegate( AdColonyAd info );
//Delegate Property
public static VideoFinishedWithInfoDelegate OnVideoFinishedWithInfo;
```

---
####OnV4VCResult####
Your class can define a method matching this delegate signature and assign it to the corresponding property. Notifies your app after an ad is displayed when a virtual currency transaction has completed.

```cs
public delegate void V4VCResultDelegate( bool success, string currencyName, int amount );
//Delegate Property
public static V4VCResultDelegate OnV4VCResult;
```
**Parameters**
* *success*
  * Whether the transaction succeeded or failed.
* *currencyName*
  * The name of currency to reward.
* *amount*
  * The amount of currency to reward.

**Discussion**
In your implementation, check for success and implement any app-specific code that should be run when AdColony has successfully rewarded. Client-side V4VC implementations should increment the user's currency balance in this method. Server-side V4VC implementations should contact the game server to determine the current total balance for the virtual currency.

---
####OnAdAvailabilityChange####
Your class can define a method matching this delegate signature and assign it to the corresponding property. Notifies when ad availability changes in any of the zones you passed to `AdColony.Configure`

```cs
public delegate void AdAvailabilityChangeDelegate( bool available, string zoneId );
//Delegate Property
public static AdAvailabilityChangeDelegate OnAdAvailabilityChange;
```
**Parameters**
* *available*
  * Whether or not ads have become a available in the zone.
* *zoneId*
  * The ID of the Zone in which ad availability has changed.

---
####IAPEgnagementType####
Type of IAP engagement for IAP Promo Ads

```cs
public enum IAPEngagementType {NONE, OVERLAY, END_CARD, AUTOMATIC}
```

---
####AdColonyAd####
class containing information about an AdColonyAd that has been shown

```cs
public members and methods:
bool shown
bool iapEnabled
string productID
IAPEngagementType iapEngagementType

string toString()
```


---
