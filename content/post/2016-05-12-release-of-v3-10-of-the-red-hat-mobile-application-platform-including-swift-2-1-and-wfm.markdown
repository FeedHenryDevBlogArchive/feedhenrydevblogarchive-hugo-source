---
author: conor.oneill
categories:
- Developers
comments: false
date: 2016-05-12T18:05:41Z
link: http://www.feedhenry.com/release-of-v3-10-of-the-red-hat-mobile-application-platform-including-swift-2-1-and-wfm/
slug: release-of-v3-10-of-the-red-hat-mobile-application-platform-including-swift-2-1-and-wfm
title: Release of v3.10 of the Red Hat Mobile Application Platform including Swift
  2.1 and WFM
url: /release-of-v3-10-of-the-red-hat-mobile-application-platform-including-swift-2-1-and-wfm/
wordpress_id: 6091
---

This post was [originally published](http://developerblog.redhat.com/) on Red Hat Developers, the community to learn, code, and share faster. To read the original post, click [here](http://developers.redhat.com/blog/2016/05/12/announcing-v3-10-of-the-red-hat-mobile-application-platform-including-swift-2-1-and-wfm/).

We have just completed the deployment of the Red Hat Mobile Application Platform v3.10 to all our actively updated grids.



The main features of this release are:




    
  * iOS SDK now has full [Swift 2.1](https://developer.apple.com/swift/) support

    
  * First Tech Preview release of our Field Workforce Management (WFM) modules

    
  * Forms performance improvements, bug fixes and import/export





## Swift SDK



Our iOS SDK now has full [Swift](https://developer.apple.com/swift/) 2.1 support. You can learn how to use it by following these links:




    
  * [Swift SDK Docs](https://docs.feedhenry.com/v3/dev_tools/sdks/ios-swift.html), with links to SDK and Sample App

    
  * [All API Docs](http://docs.feedhenry.com/v3/api.html) including code snippets



The following project templates now have Swift App options:



    
    * Blank - Minimal starting point

    
    * Hello World - Simple Client and Cloud

    
    * Push - Push Notifications

    
    * Data Sync - Offline and Data Sync

    
    * SAML- Starting point for your SAML integrations






Both the Data Sync Framework and the Unified Push Server are now fully supported in Swift.

The Build Farm is using [Xcode 7.2 and Swift 2.1](https://developer.apple.com/swift/resources/). Note that some whilst most Swift 2.2 syntax will work, some 2.2 syntax not work on our Build Farm and we will update to Xcode 7.3 in a later release to support full 2.2.



**IMPORTANT NOTE:** Customers who use their own Enterprise Certificates  to build Swift applications will have to check if the certificate has the Organizational Unit field within the subject name section. This is not the same as “Organization”. You will have to revoke and reissue a new Apple enterprise certificate for distribution. [See Apple documentation](https://developer.apple.com/support/certificates/expiration/) around this.



## WFM Modules - Tech Preview



Red Hat Mobile WFM modules are a set of more than 20 reusable npm modules (JavaScript and Node.js) for building field mobile workforce management solutions.



These modules cover the top features for a field workforce management multi-platform mobile solution and include integration with the Red Hat Mobile Application Platform (RHMAP) Forms Builder, where apps can easily use forms built within RHMAP.



Some of the modules include:




    
  * fh-wfm-workorder - For work orders

    
  * fh-wfm-sync - For data synchronization

    
  * fh-wfm-schedule - For job scheduling

    
  * fh-wfm-vehicle-inspection - For vehicle inspections

    
  * fh-wfm-mediator - An implementation of the [mediator pattern](https://addyosmani.com/largescalejavascript/)





The modules come with a sample mobile application and web portal based on [Angular.js](https://angularjs.org/), using Material Design,  which you can use to quickly bootstrap your own developments and create solutions like this:

[![wfm01](/wp-content/uploads/2016/05/wfm01.jpg)](/wp-content/uploads/2016/05/wfm01.jpg)



[![wfm06](/wp-content/uploads/2016/05/wfm06.png)](/wp-content/uploads/2016/05/wfm06.png)

Our [Getting Started Guide](https://github.com/feedhenry-staff/wfm-doc/blob/master/Getting-Started.md) explains how to setup and use the sample application.



You can see the [full set of modules on NPM here](https://www.npmjs.com/search?q=fh-wfm). Each one can be installed as usual via the npm command line.



As this is a Tech Preview, we will be iterating the functionality and documentation with your feedback and input. You can expect to see more blog posts and videos about WFM shortly. You can read the [current release notes here](https://github.com/feedhenry-staff/wfm-doc/blob/master/Release-Notes.md).



## Forms Builder






    
  * Import/Export buttons for Forms

    
  * FHC Command to import/export forms

    
  * Performance improvements on Forms Submission viewing with large number of submissions

    
  * Performance improvements on Rules Engine for complex Forms




