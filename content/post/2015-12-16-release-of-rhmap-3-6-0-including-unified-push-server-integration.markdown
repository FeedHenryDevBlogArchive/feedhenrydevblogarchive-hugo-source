---
author: conor.oneill
categories:
- Developers
comments: false
date: 2015-12-16T12:44:19Z
link: http://www.feedhenry.com/release-of-rhmap-3-6-0-including-unified-push-server-integration/
slug: release-of-rhmap-3-6-0-including-unified-push-server-integration
title: Release of RHMAP 3.6 including UnifiedPush Server integration
url: /release-of-rhmap-3-6-0-including-unified-push-server-integration/
wordpress_id: 5806
---

Version 3.6 of the Red Hat Mobile Application Platform has been released to all of our actively-updated production clusters and to the [Community Edition](http://openshift.feedhenry.com/). The main focus of this release was the integration of the Aerogear UnifiedPush Server into the platform. This means you now have iOS, Android and Windows Phone Push Notification capabilities inside RHMAP.







We also:




    
  * Added Windows 10 support, including Hybrid Apps, Native Apps and Forms Builder Apps

    
  * Added Forms Builder support to [openshift.feedhenry.com](http://openshift.feedhenry.com/).

    
  * Closed over 350 JIRA tickets including Forms bug-fixes and Community Edition bug-fixes

    
  * Removed our old Cloud Plugins lists





## UPS



Several months ago, we introduced you to the UnifiedPush Server on RHMAP, leveraging our MBaaS Services along with an OpenShift instance of UPS. We're really pleased to now have UPS as a fully integrated component of RHMAP. This integration includes a full admin console inside the platform and updated SDKs for Native iOS, Native Android, Native Windows Phone and Cordova.

This diagram shows how it all connects together in the platform:

[![push_notifications](/wp-content/uploads/2015/12/push_notifications-1024x692.png)](/wp-content/uploads/2015/12/push_notifications.png)





UPS enables you to send native push messages to different mobile operating systems. We currently support:











    
  * [Apple’s APNs](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)

    
  * [Google Cloud Messaging](https://developers.google.com/cloud-messaging/)

    
  * [Microsoft’s Push Notification service (MPNs)](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx)






If you want to get going now, you should first read our [Introduction to Push](https://docs.feedhenry.com/v3/product_features/push_notifications.html) and then dig into our [End-to-End Tutorial](https://docs.feedhenry.com/v3/guides/using_push_notifications.html). Expert users can also study the updated [Push APIs](https://docs.feedhenry.com/v3/api/api_push.html).

One of our UPS developers,  Matthias Wessendorf, has created an excellent walkthrough of the integrated functionality, using iOS as the example.



Note that we have kept the original [Services-based template](https://docs.feedhenry.com/v3/guides/app_development_push.html) for Push in the Platform and now refer to it as the Community version. This is for users who wish to make use of the upstream [AeroGear Push](https://aerogear.org/push/) Open Source project which may have features that have not yet been productized. However please note that this is a Community-supported solution only.



## 





## Windows 10



The platform APIs, SDKs and Template Apps now support Windows 10 in addition to Windows 8.1. You can use Visual Studio to locally build both Native and Cordova Apps for Windows 10 that work with the Mobile Application platform through our SDKs.

You can read all about our Windows support [here](https://docs.feedhenry.com/v3/guides/windows_platform_support.html).

We have also added Forms App support for Windows 1o so you can build Universal Windows 10 Apps with Forms functionality. You can learn how to use this functionality in [our tutorial](https://docs.feedhenry.com/v3/guides/windows_forms_apps_tutorial.html).

Note that our Build Farm currently does not support Windows 8.1 or Windows 10 for Microsoft licensing reasons.



## Platform Version Number and Grid Name



It's a small thing but oft-requested. If you look in the bottom right hand corner of the Studio, you will now see both the current RHMAP version and the Grid (instance) of RHMAP you are using. If you ever have to open a support ticket, please quote both.



## Community Edition using OpenShift



We have added Forms Builder support to OpenShift._ ___Note this requires some extra setup compared to non-OpenShift grids__. You can find [the instructions for that here](https://openshift.feedhenry.com/docs/guides/forms_support_openshift.html). Community Edition users have already been notified of this release.

We have also added information about how to get Community Support on [openshift.feedhenry.com](http://openshift.feedhenry.com) including an updated email template for new users. For reference, that Community Support is:




    
  * [#feedhenry](irc://irc.freenode.net/feedhenry) on Freenode IRC

    
  * The [redhat-mobile-open-access-users](https://www.redhat.com/mailman/listinfo/redhat-mobile-open-access-users) mailing list



There were also bug fixes on the Community Edition around Data Browser and Logging.



## Cloud Plugins Removal



In the past, back when [NPM](https://www.npmjs.com/) had _only_ 50,000 modules we had a list of recommended commonly used Node modules in the platform which we called "Cloud Plugins". As NPM has continued to grow and improve, this list has become redundant and some found it confusing. For these reasons, we have removed this section from the Studio.
