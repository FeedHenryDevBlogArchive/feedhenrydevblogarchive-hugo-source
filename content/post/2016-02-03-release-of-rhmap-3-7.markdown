---
author: conor.oneill
categories:
- Developers
comments: false
date: 2016-02-03T17:17:36Z
link: http://www.feedhenry.com/release-of-rhmap-3-7/
slug: release-of-rhmap-3-7
title: Release of RHMAP 3.7 including API Mapper preview
url: /release-of-rhmap-3-7/
wordpress_id: 5925
---

Hot on the heels of our 3.6 releases just before Christmas, we have completed our roll-out of version 3.7 of the Red Hat Mobile Application Platform to all of our actively updated grids.

The main points of interest in this release are:




    
  * Build Farm and SDK support for Android M(arshmallow), 6.0, API 23

    
  * Support for Cordova CLI 5.x on Android

    
  * API Mapper Service Preview

    
  * Cocoapods Support and Template

    
  * Example templates for SAML-based applications

    
  * Over 100 bugs were fixed

    
  * Removal of MonkeyTalk, Urban Airship Push and Flurry Analytics





## Android M(arshmallow), 6.0, API 23



You can now use the latest version of Android in both your Native and Full Cordova Apps in RHMAP. This includes support for the new [fine-grained permissions system](http://developer.android.com/training/permissions/requesting.html). You can see the full list of Android 6.0 changes (not specific to RHMAP) [here](http://developer.android.com/about/versions/marshmallow/android-6.0.html).

Our Native Android templates now use targetSdkVersion 23 and minSdkVersion 10 (Android 2.3.3).

Note that Forms Apps and Cordova Light Apps will continue to use API 19 and Cordova 3.x for the moment. A later release of RHMAP will bring both of these, along with the plugins they use, fully up to date too.



## Cordova CLI 5.2.0 for Android



With this release, we have upgraded our Build Farm to use [Cordova CLI 5.2.0](http://cordova.apache.org/docs/en/latest/guide/cli/index.html). This means you can now specify any version of Cordova Android from 3.5.1 to 5.0.0 in your full Cordova Apps.

If you don’t specify a Cordova version in your Apps, we will build your applications using the Cordova platform versions that are pinned to 5.2.0. The version used for Android is Cordova-Android 4.1.x

In addition, the Build Farm now respects the Cordova's config.xml file as a way of specifying [platform versions and plugins](http://cordova.apache.org/docs/en/latest/platform_plugin_versioning_ref/index.html).

Note that Cordova CLI for iOS will be updated in RHMAP 3.8. Also note again the comment above about Cordova Light and Forms Apps.



## API Mapper Service Preview



We’re really pleased to give you a Preview of our new API Mapper Service. This simple-looking but extremely powerful tool provides the following features:




    
  * Interact with your back-end APIs

    
  * Create GET/POST/PUT/PATCH/DELETE/HEAD/OPTIONS requests

    
  * Set headers

    
  * Save your Requests

    
  * Create a Mapping from a Request

    
  * Map fields (Include/Exclude/Rename)

    
  * Carry out complex data Transforms using custom Node.js code

    
  * Mount mappings to specific end-points

    
  * Make mappings accessible to your Cloud Code

    
  * Automatically generate Node.js code and Curl requests





[![API Mapper](/wp-content/uploads/2016/02/API-Mapper.png)](/wp-content/uploads/2016/02/API-Mapper.png)

The fact that the API Mapper runs in your MBaaS means that, if you have VPN connectivity from RHMAP to your back-end systems, it can access those systems.

As it’s a Preview, there are a few simple setup steps required. But once they are done you’ll be able to make use of all the initial features. [Our Docs](http://docs.feedhenry.com/v3/guides/api_mapper.html) have the details on getting started.

We have a lot more functionality planned for API Mapper and welcome all your feedback on things you’d like to see. Look out for a more detailed post about API Mapper in the coming days.



## Cocoapods Support and Template



We now have support for [Cocoapods](https://cocoapods.org/) in the Build Farm and have added our first Cocoapods-based template to the Studio.

[![cocoapods](/wp-content/uploads/2016/02/cocoapods.jpg)](/wp-content/uploads/2016/02/cocoapods.jpg)

In RHMAP 3.8, all templates should be available in Cocoapods.



## SAML Templates



We’ve had various requests for example templates showing how to use [SAML](https://wiki.oasis-open.org/security/FrontPage) with RHMAP. We have added Client and Cloud templates for the following:




    
  * Cordova

    
  * iOS

    
  * Android

    
  * Windows



[![saml01](/wp-content/uploads/2016/02/saml01.jpg)](/wp-content/uploads/2016/02/saml01.jpg)

These can easily be wired up to (for example) an ADFS instance to see the entire flow working. Learn all about the functionality and templates in [our Docs](http://docs.feedhenry.com/v3/guides/using_saml_for_authentication.html).



## Removals



As [previously announced](http://www.feedhenry.com/updates-industry-releases-related-to-mobile-oses-sdks-and-tools-on-rhmap/), we have removed the following SDKs from our Build Farm. You can continue to manually include them in your projects if you wish.




    
  * MonkeyTalk

    
  * Urban Airship Push

    
  * Flurry Analytics


