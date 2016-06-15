---
author: craig.brookes
categories:
- Developers
comments: true
date: 2013-08-27T08:48:57Z
link: http://www.feedhenry.com/feedhenry-now-supports-multiple-versions-of-node-js/
slug: feedhenry-now-supports-multiple-versions-of-node-js
title: FeedHenry now supports multiple versions of Node.js
url: /feedhenry-now-supports-multiple-versions-of-node-js/
wordpress_id: 2295
---

Node.js is a fast moving and cutting-edge server side platform. Updates to it happen often. In fact, just take a look at  [their blog ](http://blog.nodejs.org/) to see how many changes are happening and how fast.

At FeedHenry, we use Node.js for many of the components which make up our platform, including those that manage the Node.js hosting platform. As developers we always want to keep up with the latest releases. The reasons for this are twofold: firstly we are developers and we enjoy using the new features and improved performance that often come with an update, and secondly as we are offering a Node.js platform on which you can host your apps, it is important that we fully understand everything that is happening in the Node world.


## Node v0.6


For quite some time the only Node.js runtime that we have been able to offer in our hosted environment has been Node 0.6. While this was current at the time, it is now significantly out of date; so much so, that several well known modules have dropped support for it.

However, we didn’t want to simply change the version of node across the board and not give our customers any choice in the matter. So we took this as an opportunity to develop a feature into the FeedHenry platform that allows our customers to choose which version of Node.js they want to use on an application by application basis.


## Node v0.6 / Node v0.8 / Node v0.10


We are pleased to announce that you can now choose from Node v0.6, Node v0.8 and Node v0.10 when deploying your application to the FeedHenry platform. This can be done in two ways. Firstly through the FeedHenry AppStudio, where you can choose from a clear drop down when deploying your App and secondly with the fhc command line tool using the stage command.

You can read more about this on [our documentation site.](http://docs.feedhenry.com/v2/set_node_vers.html)

The default version of Node.js will now be v0.8. We highly recommend that you test your app against this version of Node.js locally using fhc local or using the dev version of your app before deploying your live app. If you don’t wish to change to the latest version of Node.js, then simply choose one of the earlier versions from the dropdown when deploying. All existing apps will continue to run on the version of node they were deployed with (i.e. Node v0.6) until such time as they are re-deployed against a different version of Node. We have no plans to deprecate support for Node v0.6 at this time, so there is no need to re-stage any apps unless you wish to take advantage of the features and benefits that later versions of Node offer.


### What does this mean for Developers and Enterprises?


Being able to clearly see and choose the underlying version of Node.js that your application is deployed on means that you can safely lock down your module dependencies and be sure they will work on the version of node that your app is running on. Having a development environment that closely mirrors the environment that your application is going to be running on within FeedHenry,  allows for the reduction of unexpected bugs and faster, more thorough error resolution. Because you have control, if your app is running on Node v0.6 for example and you found that you needed a module that required Node v0.8 then you could decide to move your application to that runtime and take advantage of that module.


