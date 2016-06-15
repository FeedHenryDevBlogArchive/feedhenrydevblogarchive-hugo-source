---
author: conor.oneill
categories:
- Developers
comments: false
date: 2015-11-02T17:05:15Z
link: http://www.feedhenry.com/updates-industry-releases-related-to-mobile-oses-sdks-and-tools-on-rhmap/
slug: updates-industry-releases-related-to-mobile-oses-sdks-and-tools-on-rhmap
title: Updates & industry releases related to mobile OSes, SDKs and tools on RHMAP
url: /updates-industry-releases-related-to-mobile-oses-sdks-and-tools-on-rhmap/
wordpress_id: 5743
---

By: Conor O'Neill, Product Manager, Red Hat Mobile

There are a range of updates and industry releases related to various mobile Operating Systems (OSes), Software Development Kits (SDKs) and tools used in the Red Hat Mobile Application Platform (RHMAP) that we wish to let you know about.

_It's important to note that no changes or actions are necessary by you for any currently installed apps on existing devices._



## Summary






    
  1. The release of iOS 9 by Apple

    
  2. The release of Android M by Google

    
  3. The upgrade of our Build Farm to support Android API Level 22 (5.1, Lollipop)

    
  4. The deprecation by the Apache Cordova team of the Cordova Registry

    
  5. The removal of BlackBerry and Windows Phone 7.x Support from RHMAP

    
  6. The addition of Windows 8.1 Universal App support to RHMAP

    
  7. The removal of some older unused integrations from RHMAP







## iOS 9



Apple recently released iOS 9 and Xcode 7 which has some changes related to security which may impact your Apps.



### Where no action is required






    
  1. Existing iOS Apps using any version of our iOS SDK will continue to run as before on an iOS 9 device

    
  2. Existing iOS Apps using any version of our iOS SDK installed on an iOS 8 device will continue to run as before after the device is upgraded to iOS 9





### iOS 9 Mandatory bundle identifier on RHMAP Enterprise App Store



iOS 9 now requires that you specify the bundle identifier in what is called the plist file. If you do not specify it, then apps will not install from the RHMAP Enterprise App Store on iOS 9 devices.

You can set the bundle identifier in the RHMAP Studio at Projects > Your Project > Client App > Config page under the relevant iPad/iPhone/iOS Universal section. This value needs to be copied to the “iPhone Bundle ID” field for individual binaries in Admin > Mobile App Management > App Store Apps > Binaries.

Once this is done, the correct plist file will be generated when you download the app from the store and this will allow the app to install on iOS 9.



### Running Xcode 7 locally and targeting iOS9



When building an app with Xcode 7 and base SDK iOS 9 on your local workstation, for installation on an iOS 9 device, you must:




    
  1. Switch "Enable Bitcode" to off in build settings.

    
  2. Check if you use NSURLSession to make HTTP calls instead of the provided RHMAP classes. If you do, and if you are not on a TLS 1.2 compatible RHMAP grid, you'll need to [define a plist exception](https://forums.developer.apple.com/thread/4017). (see note below)

    
  3. The same rule applies if you are making calls directly to other 3rd party services

    
  4. Check if you are using UIWebView (i.e. Cordova). If you are, and if you are not on a TLS 1.2 compatible RHMAP grid, you'll need to [define a plist exception](https://forums.developer.apple.com/thread/4017).



NOTE: All of our AWS grids are TLS 1.2 compatible and our LON3 grid in the UK will be compatible by November 5th. Customers who run dedicated installations of RHMAP on TEF Instant Server based grids must make the plist exception changes listed above.



### Upcoming addition of iOS 9 Support & removal of iOS 6 Support



The RHMAP Build Farm does not currently support the targeting of iOS 9. Apps built in the Build Farm will continue to target iOS 8 until the relevant upgrade is complete. Once iOS 9 and Xcode 7 are added, they will become the defaults in the Build Farm.

In keeping with our long-standing policy of tracking Apple’s OS support (CurrentVersion / CurrentVersion-1 / CurrentVersion-2), this means that iOS 6 will no longer be supported in the Build Farm.

We currently expect this upgrade to occur in v3.8.0 of RHMAP in early 2016.



## Android 5.1, Lollipop, API Level 22 support



In the upcoming v3.5.4 release of RHMAP, we will be changing the default version of Android that we target in our Apps from Android API 19 (Android 4.4, KitKat) to Android API 22 (Android 5.1, Lollipop). This also means that the Build Farm will be able to support Gradle builds from Android Studio in addition to the existing Ant builds from ADT.

All current apps should continue to work as before and no action is necessary on your part for those apps.

However, if you wish, you will be able to target Android API 22 and make use of any newer functionality available there.

Once the update has been completed in the platform, Cordova Light Apps and Forms Apps will continue to default to Android 19 for the moment. Native Android Apps and Full Cordova Apps will be able to specify any API level <= 22.



## Android 6.0, M, Marshmallow, API Level 23 topics



Google recently released Android M which has some changes that may impact your Apps. Note that M is only available at the moment on a small number of Nexus devices and one other LG device.




    
  * Existing App binaries built using our Android SDK that target versions of Android API lower than 23 will continue to run unchanged on devices that are updated to Android M. The same is true for new devices running Android M.

    
  * The current version of our Android SDK (v2.x.x) is not compatible with Apps that are built locally on your workstation in Android Studio and target Android M (Android API Level 23).

    
  * The next major version our Android SDK (v3.x.x) will be compatible with Apps that are built locally on your workstation in Android Studio and target Android M (Android API Level 23). The release date for this version will be announced soon.

    
  * If you have any Android app (not just those using our SDKs) that makes use of the older Android HttpClient APIs, and you wish to target Android M, then you will need to update those apps to use [HttpUrlConnection](http://developer.android.com/reference/java/net/HttpURLConnection.html) (or [OKHttp](http://square.github.io/okhttp/)), or include the [HttpClient support library](http://developer.android.com/about/versions/marshmallow/android-6.0-changes.html) from Google.

    
  * The RHMAP Build Farm also does not currently support the targeting of Android M. We will announce when Android M support is available.





## Cordova Registry



The Apache Software Foundation [announced in April](https://cordova.apache.org/announcements/2015/04/21/plugins-release-and-move-to-npm.html) that they were planning to shut down the Cordova Registry on October 15th and move to NPM for plugins. Whilst it appears this shutdown has been delayed, you should ensure any of your Hybrid Apps where you have specified extra Cordova plugins are updated. Red Hat Mobile developers who make use of our config.json file will need to follow [the existing instructions here](http://docs.feedhenry.com/v3/guides/using_cordova_plugins.html) to point to the GitHub URLs for the plugins instead of the Cordova Registry.

All of the plugins used by default in Red Hat Mobile Templates have already made this move so if you are just using our default set of plugins in Cordova Light Apps, Cordova Apps or Forms Apps, then no action is necessary.

A future release of of RHMAP will fully support the use of NPM for Cordova plugins.



## Removal of BlackBerry and Windows Phone 7.x Support



Due to the ever-shrinking market share of BlackBerry and the complete lack of use of this target in our platform, we are removing all support for it with immediate notice. Our JS SDKs may continue to work on more modern BlackBerry devices but this is not supported in the product.

As Microsoft ceased support for Windows Phone 7.x in 2014 and we have no active customers using this target, we are removing all support for it with immediate notice too.



## Windows 8.1 and Windows 10 Universal Apps



Due to licensing changes in Visual Studio, we currently cannot provide Build Farm support for Windows 8.1 or 10 Universal Apps. Windows 8.1 is supported in our Hybrid JS and Native .NET SDKs, which can be built locally on a developer machine. The same support for Windows 10 will be available in v3.6.x of RHMAP by the end of November.



## Removal of older unused integrations



We have a variety of old integrations built into the platform which have not been used by customers for a very long time. We have therefore decided to remove these SDKs from the Build Farm with immediate effect.

The removals are as follows:




    
  * MonkeyTalk

    
  * Urban Airship Push

    
  * Flurry Analytics



In every case you can still use these products with RHMAP by manually adding their latest SDKs to your projects and creating custom MBaaS Connectors.

Note that we are also replacing Urban Airship Push with an integrated version of the Red Hat Unified Push Server in the 3.6.0 release of RHMAP.
