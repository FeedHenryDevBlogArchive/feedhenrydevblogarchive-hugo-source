---
author: conor.oneill
categories:
- Developers
comments: false
date: 2016-06-17T14:52:49Z
link: http://www.feedhenry.com/upcoming-deployment-of-version-3-11-of-the-red-hat-mobile-application-platform/
slug: upcoming-deployment-of-version-3-11-of-the-red-hat-mobile-application-platform
title: Upcoming deployment of version 3.11 of the Red Hat Mobile Application Platform
url: /upcoming-deployment-of-version-3-11-of-the-red-hat-mobile-application-platform/
wordpress_id: 6123
---

# **IMPORTANT: POTENTIAL BREAKING CHANGES BELOW.**

  We will begin deploying v3.11 to our actively updated grids in the next 10 days. The main features of this release are:

*   Tech preview of Node.js 4.4.2 LTS as an option for your Cloud Code
*   Google Play Store changes (Possible Action Required)
*   Updating Forms Apps to new versions of Cordova and Cordova Plugins
*   Updating Cordova “Light” Apps to new versions of Cordova and Cordova Plugins
*   Announcement of  future deprecation of Node.js 0.8.x
*   Updating Xamarin support to v5.x
*   Addition of options for CSV and PDF in Forms Submissions retrieval via API

## Node.js 4.4.2 LTS Tech Preview

When you deploy your Cloud Code or Services, you can now select Node 4.4.2 LTS as the target version, in addition to the existing Node 0.10.x. Note that 0.10.x remains the default in this release. Re-deployment of existing code will also default to 0.10 unless you manually select 4.4.2. Whilst 4.4.2 is fully functional for your code, we are calling it a Tech Preview as not all of our templates have been fully ported to this version.  

## Google Play Store changes (Possible Action Required)

It is important to note that Cordova apps of any kind, built with Cordova < 4.1.1 will be rejected by the Google Play Store after July 11 2016\. This affects Cordova, Cordova Light and Forms Apps on RHMAP, whether you build them locally or on our Build Farm. If you wish to put those apps in the **public** Google Play Store after July 11, you must update those Apps to Cordova > 4.1.1 or they will be rejected the next time you try to upload a new version. See [Google FAQ here](https://support.google.com/faqs/answer/6325474?hl=en). If you wish to update your Apps, you should do the following:

*   Cordova Apps - Please check your Cordova config.xml. If you are specifying a version of Cordova there, it must be updated to be 4.1.0 (this strangely means 4.1.1 to Cordova) or greater. E.g.

    <pre><engine name="android" spec="~5.2.0" /></pre>

    If it’s not specified, you don’t need to change that file. Note: You will have to test any plugins you use to ensure they are Cordova 5.2 compatible and update as needed. Then rebuild your app in the Build Farm and upload it to the Google Play Store.
*   Light Apps - Once you rebuild your Light App in the RHMAP 3.11 Build Farm, it will automatically use Cordova 5.x. The older versions of our default plugins ([see list here](http://docs.feedhenry.com/v3/guides/using_cordova_plugins.html#using_cordova_plugins-considerations_for_cordova_light_apps)) have been tested to work with Cordova 5.2.Note: If you have added other plugins to config.json, you will need to test if they still work with 5.2 and possibly update them to the latest versions.
*   Forms Apps - Once you rebuild your standard auto-generated Forms App in the RHMAP 3.11 Build Farm, it will automatically use Cordova 5.x.Note: If you have customised the default app and added other plugins to config.json, you will need to test if they still work with 5.2 and possibly update them to the latest versions.

## Latest Cordova for Forms and Light Apps

In our recent releases we updated our Cordova support for standard Cordova Apps to Cordova CLI 5.2\. We are now updating both our Forms Apps and Cordova Light Apps to the same level for iOS, Android and Windows. In a tiny number of cases, this can break some Cordova Light Apps and you need to ensure you follow the update procedure below, only if this applies to you.

### Forms Apps

You should see no functional difference in the auto-generated Forms Apps with this update. Existing Forms Apps do not need to be changed. On your next Build Farm build, they will automatically use Cordova CLI 5.2. New Forms Apps will automatically use Cordova CLI 5.2 in the Build Farm and no action is required on your part.

### Cordova Light Apps (Potential Breaking Change)

Cordova Light Apps are specific to Red Hat Mobile and provide a simple way to generate Cordova Apps using just HTML/CSS/JS. All of the Cordova pieces are handled for you by our Build Farm. Existing Cordova Light Apps will continue to run without change. The vast majority of existing Cordova Light Apps will continue to build in our Build Farm without change. However, if you have Cordova Light apps, you must check the following (This does not apply to standard Cordova Apps):

1.  If your App is a Cordova Light App (you can see its type in the Project in the Studio)
2.  And if it is missing the www/config.json file
3.  And if you are using one of a set of Cordova plugins [listed here](https://access.redhat.com/articles/2361891)
4.  And if you are going to re-build your application
5.  Then you need to create a www/config.json file  and update your code, [using the instructions here](https://access.redhat.com/articles/2361891)

Again note that it is extremely unlikely that your app meets all of the above conditions, but you should check to be absolutely sure. If you are creating any new Cordova Light Apps from scratch, you must include config.json. If you start with any of our Cordova Light templates, you will get this by default.

## Future deprecation of Node.js 0.8.x

Note that Node.js 0.8 is still selectable as a target version in this release. However it has not been the default for over two years in RHMAP. This version of Node.js has been long obsolete and is not supported by the Node.js Foundation or by Red Hat Software Collections. For all of these reasons, we will be removing support for Node.js 0.8 by September 30 2016. It is very unlikely you are using Node.js 0.8, but if you are, you will need to carry out any porting work to update it to Node.js 4.4.2 by this time. If your app does not need any changes, or never needs a clean redeployment, an upgrade of the Node.js version is not necessary, but is recommended.

## Xamarin

We have updated our Xamarin support to v5.x and refreshed the templates appropriately. Note that Push functionality in the SDK is still in development and won’t be released in 3.11\. Xamarin Apps must still be built locally and cannot use the Build Farm.

## Forms

* Forms Submissions retrieval via API now offer options for CSV and PDF. API details will be [here](http://docs.feedhenry.com/v3/api/api_forms.html#api_forms-node_js_api).
* Further performance improvements in submissions display in Studio with very large numbers of submissions

## Known Issues

*   Forms and broken Environments - If you have Environments that are no longer accessible, you will get errors from the Forms section of the Studio. Please contact Red Hat Support to fix the problem.