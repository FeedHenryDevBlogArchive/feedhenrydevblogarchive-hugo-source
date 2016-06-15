---
author: john.frizelle
categories:
- Developers
comments: false
date: 2014-08-29T08:36:01Z
link: http://www.feedhenry.com/extending-cordova-mobile-spec-tester-app-cloud/
slug: extending-cordova-mobile-spec-tester-app-cloud
tags:
- cordova
- FeedHenry 3
- mobile application platform
title: Extending the Cordova Mobile Spec Tester App into the Cloud
url: /extending-cordova-mobile-spec-tester-app-cloud/
wordpress_id: 4700
---

Here at FeedHenry we're big fans of [Apache Cordova](http://cordova.apache.org/) - the leading Hybrid App Development Framework - and have been supporting it in our Mobile App Platform for several years now. Over that time, we have upgraded our supported version of Cordova many times (we first started supported cordova at version 0.9). One of the core parts of the FeedHenry product offering is our Cloud Hosted [Build Farm](http://www.feedhenry.com/mobile-application-platform/app-development/) which "...provides builds for native and hybrid apps for iOS, Android, Windows Phone and Blackberry, generating the build and digitally signing the apps". As we maintain and upgrade the Build Farm - to support new versions of Cordova, new versions and flavours of mobile Operating Systems (iOS 8 support Coming Soon) - a crucial task is to ensure that we do not break support for any device APIs. To assist with this, we have found the [Cordova Mobile Spec App](https://github.com/apache/cordova-mobile-spec) to be an excellent tool. It is "a set of automated & manual tests that test Cordova core functionality" which allow us to manually verify API functionality on various devices and mobile OS versions pre and post upgrade. However, there are a few limitations to this approach:



	
  * Testing the full API set is a somewhat manual process

	
  * There is no information/history of previous tests - details of test failures need to be manually recorded

	
  * There is no statistical information available on which tests fail most often on which mobile operating systems




By using the FeedHenry platform, we were quickly able to deal with these limitations and turn a stand alone app into one part of a Cloud Powered Mobile Solution. Here's what we did: First, we forked the Mobile Spec App into the GitHub organisation we use for [template apps](https://github.com/feedhenry-templates/fh-mobile-spec-app) and added the [FeedHenry Cordova SDK](http://docs.feedhenry.com/v3/dev_tools/sdks/cordova.html). This allowed the mobile app to communicate with the FeedHenry Cloud.

[![2014-08-27 12.00.11](/wp-content/uploads/2014/08/2014-08-27-12.00.11-168x300.png)](/wp-content/uploads/2014/08/2014-08-27-12.00.11.png)



Next, we created a standard [FeedHenry  Node.js Cloud Application](https://github.com/feedhenry-templates/fh-mobile-spec-cloud) for the Mobile Spec App to submit it test results to. [![App Cloud API Docs](/wp-content/uploads/2014/08/App-Cloud-API-Docs-947x1024.png)](/wp-content/uploads/2014/08/App-Cloud-API-Docs.png)



Then, we added a [Jasmine test runner](http://jasmine.github.io/) to the Mobile App to allow the full suite of API test to be run automatically & uploaded the results to the Node.js Cloud App using the standard [$fh.cloud() API call](http://docs.feedhenry.com/v3/api/api_act.html).

![2014-08-27 12.00.59](/wp-content/uploads/2014/08/2014-08-27-12.00.59-168x300.png)



Finally, we created a [FeedHenry Web Portal](https://github.com/feedhenry-templates/fh-mobile-spec-portal) (also powered by Node.js) to allow the test results from individual runs of the app on any mobile device to be viewed, sorted, searched, aggregated and graphed.



[![Screenshot 2014-08-25 17.44.23](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.44.23-300x121.png)](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.44.23.png)   [![Screenshot 2014-08-25 17.44.41](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.44.41-300x160.png)](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.44.41.png)

[![Screenshot 2014-08-25 17.45.08](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.45.08-300x160.png)](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.45.08.png)   [![Screenshot 2014-08-25 17.45.27](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.45.27-300x158.png)](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.45.27.png)

[![Screenshot 2014-08-25 17.45.46](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.45.46-300x145.png)](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.45.46.png)   [![Screenshot 2014-08-25 17.46.25](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.46.25-300x151.png)](/wp-content/uploads/2014/08/Screenshot-2014-08-25-17.46.25.png)



The result -  a full mobility project for automated Cordova API Testing which persists, aggregates and visualises test data for the Cordova Mobile Spec App. This project is available as one of our standard FeedHenry 3 templates, so feel free to take a look and use it for your own automated testing.

[![Screenshot 2014-08-27 12.53.12](/wp-content/uploads/2014/08/Screenshot-2014-08-27-12.53.12-300x200.png)   ![Screenshot 2014-08-27 12.53.32](/wp-content/uploads/2014/08/Screenshot-2014-08-27-12.53.32-300x124.png)](/wp-content/uploads/2014/08/Screenshot-2014-08-27-12.53.12.png)
