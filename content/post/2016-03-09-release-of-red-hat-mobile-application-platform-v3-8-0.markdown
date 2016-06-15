---
author: conor.oneill
categories:
- Developers
comments: false
date: 2016-03-09T18:33:43Z
link: http://www.feedhenry.com/release-of-red-hat-mobile-application-platform-v3-8-0/
slug: release-of-red-hat-mobile-application-platform-v3-8-0
title: Release of Red Hat Mobile Application Platform v3.8.0
url: /release-of-red-hat-mobile-application-platform-v3-8-0/
wordpress_id: 5997
---

In keeping with our recent release cadence, version 3.8 of the Red Hat Mobile Application Platform has just been deployed to all actively updated Grids.

The main features of this release are:



## iOS 9 updates






    
  * Full iOS 9 support in the Build Farm has now been added. All iOS apps are now built using Xcode 7.2 and target iOS 9.2.1 by default, including Forms Apps and Cordova Light Apps.

    
  * All of our iOS templates work without any code change on iOS9

    
  * No iOS9 specific SDK updates are needed

    
  * All Studio iOS template apps are now [based on Cocoapods](http://www.redhat.com/blog/mobile/cocoapods-and-rhmap-a-love-story/)

    
  * Note that full Swift support is coming in a later release. However you can already use our current SDK with ObjC if you are writing in mixed Swift/ObjC.





## Cordova updates






    
  * In RHMAP 3.7, we added support for the latest version of Cordova Android. We are now doing the same for Cordova iOS which is at v3.9.x and is the default for Full Cordova Apps.

    
  * Both iOS and Android now use Cordova CLI 5.2.0 which gives you access to all the latest Cordova platforms and plugins. As mentioned in the [RHMAP 3.7.0 release notes](http://www.redhat.com/blog/mobile/release-of-rhmap-3-7-including-api-mapper-preview/), you can now use Cordova’s config.xml to specify platform versions and plugins.

    
  * Forms Apps and Cordova Light App continue to use Cordova 3.x on iOS. We will be upgrading these in a later release.





## Integration with the Apperian MAM



If you make use of [Apperian](https://www.apperian.com/) for Mobile Application Management, you can now automatically send your built mobile binaries from our Build Farm to your Apperian App Store.

[![apperian_feedhenry_blog](/wp-content/uploads/2016/03/apperian_feedhenry_blog.png)](/wp-content/uploads/2016/03/apperian_feedhenry_blog.png)

Detailed blogpost and video will follow soon on this.



## Sustaining Engineering



There were a large number of bug-fixes and enhancements made in this release, including:




    
  * A major focus on migrating very large MongoDB datasets via the Studio and providing more progress information about the migration.

    
  * Adding a strict step on project deletion to avoid mistakes by users

    
  * Adding more metadata to Forms Submission Export

    
  * Fixing App Preview refresh





## Unified Push Server updates



We have updated our integrated Unified Push Server based on the [Aerogear Push](https://aerogear.org/push/) Open Source upstream project  v1.1.1. Release notes for that are [available here](https://issues.jboss.org/secure/ReleaseNote.jspa?projectId=12313724&version=12327457&_sscc=t).



## Data Sync / Offline Support updates



We updated the [Data Sync example code/snippets](http://docs.feedhenry.com/v3/api/api_sync.html) in Docs for iOS, Windows and Android.



## Known Issue(s)



When trying to download an app built through the Build Farm on an iOS 9.1 device, the message "Unable to Download App" is shown and the app can not be downloaded/installed.

This issue only affects version 9.1 of iOS. To work around it, you can either build the app locally in Xcode, or upgrade your device to an iOS version higher than 9.1.
