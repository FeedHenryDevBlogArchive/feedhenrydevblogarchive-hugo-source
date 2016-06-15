---
author: cian.clarke
categories:
- Developers
comments: true
date: 2013-08-15T08:44:18Z
link: http://www.feedhenry.com/using-cloud-plugins-to-accelerate-your-mobile-app-development/
slug: using-cloud-plugins-to-accelerate-your-mobile-app-development
title: Using Cloud Plugins to accelerate your mobile App development
url: /using-cloud-plugins-to-accelerate-your-mobile-app-development/
wordpress_id: 2287
---

As you know, your FeedHenry Cloud back-end is powered by the blazingly fast Node.js runtime. One of Node’s many advantages is the incredible number of modules available to it in the NPM repository.

The NPM Ecosystem contains (at time of writing) 37,990 modules – a mind boggling number to choose from. This includes a wide variety of developer tools, ways of integrating into third party systems, and several hundred different ways of performing a HTTP request. Here at FeedHenry, we’ve been doing Node.js for longer than most and we know which way is [best](https://github.com/mikeal/request).

But how do _you_ decide which modules to use, particularly if you are new to Node? Cloud Plugins solves this for you in a way that’s both simple and powerful, as it kickstarts your Node.js experience. Our Plugin Catalog is a curated, categorised list of best-of-breed plugins for common tasks that developers encounter in the cloud portion of their app.

![cloudplugins011](/wp-content/uploads/2013/09/cloudplugins011.jpg)

We have worked with both the plugins here and the associated services. In many cases, we are regularly using these services in production with our customers. We have also partnered with some of these companies.


## Save Valuable Development Time


The typical path to integrating with a service usually starts with an investigation of the existing modules available to you and picking whichever looks best. Then you move on to the documentation for this module, trying to assemble code to connect to the relevant service & verify connectivity. This can often be an exercise in frustration as you discover poor/non-existent documentation or serious holes in the implementation.

We’ve saved you from both these steps by not just selecting the best modules for common integrations but also by providing instructions and code to test your integration and use as a building block for deeper connectivity.

We’ve initially included Plugins for:



	
  * Databases

	
  * Messaging

	
  * Analytics

	
  * SaaS Connectors


along with eCommerce, Logging, Data Storage, File Storage, Testing, Tools and File Storage.


## Environment Variables


We’re also using one of my favourite platform features, environment variables, to configure these plugins. The configuration details you need to connect to the back-end system of your choice are configured using Environment Variables.

Environment variables allow different values to be set for ‘Dev’ and ‘Live’ targets, meaning your development and staging backend system can be configured in tandem with your development app, whilst the live version of the application  hits the appropriate production endpoints.

Environment variables also make updating configuration a breeze. Hostnames, username and password details are forever changing. Environment variables allow you to push this change in seconds, without needing to make changes to the codebase of your application.
Lastly, sensitive configuration information is no longer  stored in the codebase of your application – an important security abstraction.


## Using A Plugin


Getting started with a plugin is simple and you’ll only need to do a small amount of coding. Our snippets give you enough to get up and running with your integration quickly, but of course it’s still up to you, the developer, to do something further with the data.

In this video example, we’ve integrated with SendGrid, and illustrated how easy it is to create some cloud code to send emails.

[embed width="500"]http://vimeo.com/71270687[/embed]

We will be expanding the Cloud Plugins catalogue regularly and we value your input on which modules you’d like to see there.

_[@cianclarke ](http://www.twitter.com/cianclarke)is a software engineer at FeedHenry. Cian is leading FeedHenry’s development of mobile backend-as-a-service features, using open technologies wherever possible._




