---
author: conor.oneill
categories:
- Developers
comments: false
date: 2014-04-29T18:31:37Z
link: http://www.feedhenry.com/summary-new-developer-features-feedhenry-3/
slug: summary-new-developer-features-feedhenry-3
tags:
- feedhenry3
- fh3
title: A summary of all the new Developer Features of FeedHenry 3
url: /summary-new-developer-features-feedhenry-3/
wordpress_id: 4024
---

CONOR O'NEILL, DIRECTOR OF PRODUCT MANAGEMENT, FEEDHENRY

As you've [probably read](http://www.feedhenry.com/feedhenry-launches-new-mobile-app-platform/) here and elsewhere, we launched FeedHenry 3 today. This is the culmination of many different development efforts over the past several months and is a major leap forward for the FeedHenry platform. We've taken all the things you know and love about FeedHenry like local development, Node.js, Git integration, our build farm and Drag & Drop Apps; and turned them all up to 11. Over the next 2 weeks we'll dive into the nitty gritty of all the new and improved features here, but for today we thought you'd like to read the technical highlights.

[![project](/wp-content/uploads/2014/04/project-1024x535.jpg)](/wp-content/uploads/2014/04/project.jpg)


## Projects not Apps


As App complexity increases and back-end integrations multiply, your mobile efforts become more than a single developer building a single app. We now support multiple client apps, Node apps, web apps and services, all within one project. This means your iOS team, Android team, Cordova team and back-end team can all work independently in their own Git repos but connected together as part of one overall project.


## Native iOS, Native Android and WP8 Build Farm


Our Build Farm has always been able to build simple Cordova/PhoneGap Apps for iOS/Android/WP/Blackberry. We can now build fully Native iOS and Android Apps along with Windows Phone 8, and those Apps can be part of the same project as the Cordova ones. This means that you can automate all of your builds for all targets and Configuration Management staff can build for all platforms without needing local IDEs installed for them all.


## Import external code including Xamarin and Appcelerator


Not only can you easily import HTML5, Cordova, Node and Native iOS/Android/WP8 projects into FeedHenry 3, you can import Xamarin and Appcelerator projects too. In every case it is trivial to add the relevant FeedHenry SDK whilst we walk you through the steps.


## Build History and Credential Bundles


Your build history is accessible along with the bundles of credentials that were used to build the various versions. Our larger customers tell us that the management of binaries/certs is a particular pain point for them and they really appreciate having one central point of control in FeedHenry 3.


## Cordova Plugins


You can now bundle any Cordova plugin with your HTML5/JS/CSS/Cordova apps and build them in our Build Farm. This opens up a huge range of plugins, including those you write yourself.


## Zero Code, Drag & Drop Apps (and APIs, natch)


You can now build Forms-based Apps with zero code end-to-end inside the Studio and you can also use the new Forms APIs to easily add dynamic forms to any App. Any changes to your Forms appear in the apps the next time your users load them up, so no new versions are required in the Apps Stores. Non-developers can build Forms and content admins can be limited to only viewing submissions, ensuring that they are can't interfere with app development.


## Deep Git Integration


We realised the power of Git long before most people in this business and we've had it baked-in for years. But, as with all of our FeedHenry 3 features, we listened to developers and added exactly what they needed. Firstly we have Git hosting integrated so you don't have to rely on having a GitHub or Bitbucket account. But just as importantly, we fully support git remote, so you now have the full power of distributed version control and can push/pull changes from whereever you wish into/out-of FeedHenry. Which leads neatly to the next feature.......


## Grunt-ified local development


Whilst you can always work using the web gui and our improved Ace editor, most developers prefer to use their own local development environment and tools. Our fhc tool has always supported this to give you all the commands you need to interact with the platform. Now you also have full Grunt support in all of our templates. So you can use the power of fhc+git+grunt to develop and debug both your Cordova and Node.js code locally and then deploy it to FeedHenry. This of course opens up a wide range of possibilities with automation too.


## Full Node.js Express REST API support


FeedHenry has been doing Node since 2011, long long before the Node-washers arrived and jumped on the latest cool thing. This means we know it inside out. We've taken the pain of the early days and know how to make Node sing. We've had Express support for quite a while but we now also offer completely standard support for REST APIs in Express. So take whatever Node code you have and drop it straight into FeedHenry 3.


## Connections


Connections are an extremely powerful way of dynamically reconfiguring which Node.js Cloud App your mobile Apps are talking to without having to re-deploy your Apps. For example you can spin up a new version of your cloud code to try out a fix for Android and point all of your Android users to that code, whilst everyone else continues to use another version.


## Shared Discoverable Microservices


Rather than having unique custom cloud code for every app and every project, you should aim for maximum re-use. As Enterprises roll-out suites of Apps, many of them will want to access the same back-end systems. By spinning up standalone discoverable (via API Blueprint) Microservices inside FeedHenry 3, you can create something re-usable that your apps can share. Also, rather than exposing an entire internal system to the entire world, you can use our Services functionality to provide just a sub-set of APIs to your Apps, all proxied securely through FeedHenry.


## Web App Hosting


Web Apps are first class citizens in FeedHenry 3. You can host both static web-sites or your own Node.js web apps that can use the same APIs as your mobile apps to talk to your Node.js Cloud Apps. These Web Apps are ideal for management and admin portals to go with your mobile apps.


## Responsive touch-friendly Studio UI


New features aren't much good if they are hard to use. Our Studio UI has been re-built from the ground up. Best practice responsive design and it even works nicely on your Tablet. This new UI gives us the ability to add new functionality quickly and easily and gives you a far superior user experience. We've also carried out some major improvements in our App preview to cover a wide range of devices, device sizes and orientations.


## New templates, examples and tutorials


We're building out a great set of templates, examples and tutorials so you can be up and running with FeedHenry, using best practices, in the shortest time possible. Add in our fully integrated docs with context-aware help and you'll be productive on day 1.

Lots more detail to come, very soon!




