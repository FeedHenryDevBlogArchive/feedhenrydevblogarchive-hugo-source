---
author: conor.oneill
categories:
- Developers
comments: false
date: 2014-06-24T17:26:33Z
link: http://www.feedhenry.com/summary-new-feedhenry-3-features-fhc-command-line-tool/
slug: summary-new-feedhenry-3-features-fhc-command-line-tool
tags:
- cli
- feedhenry3
- fhc
- node
- node.js
- npm
title: A summary of the FeedHenry 3 changes in our fhc command-line tool
url: /summary-new-feedhenry-3-features-fhc-command-line-tool/
wordpress_id: 4319
---

We develop everything in FeedHenry API-first ,which means our command-line tooling is just as powerful as our Web UIs, with the added advantage of automation and scriptability.

The main FeedHenry CLI is called _fhc_ and it is how most developers interact with the platform. Version 0.30.x has had quite a few changes (to put it mildly!) for FeedHenry 3 and we'll summarise those changes here. _fhc_ will continue to work with both FeedHenry 3 and previous versions of the platform for the foreseeable future. Of course, being a Node.js App, it supports Windows, OSX and Linux.


## New FeedHenry 3 Commands




### fhc projects


Projects are a major new concept within FeedHenry 3 and enable groups of developers to work together on different but related Apps (e.g. iOS Native Client,  Cordova Client, Cloud Code and mBaaS Service). You can this command to list/create/update/read/delete or clone a project. "Clone" is particularly powerful as it does a 'git clone' of each App in your Project into the current working directory.


### fhc services


Services are one of the other main pillars of FeedHenry 3, enabling you to create standalone cloud services which are re-usable across one or more projects. This makes them ideal as connectors to your back-end systems and we are regularly expanding the list of out-of-the-box services. You can this command to list/create/update/read or delete a service.


### fhc connections


Connections provide the ability to dynamically reconfigure which Cloud Code a specific app (and app version) communicates with. This can be done without needing to release new versions of your apps. Different apps/versions can simultaneously talk to different versions of your cloud code. You can use this command to easily list and change the connections.


### fhc artifacts


In FeedHenry 3, you can now view your build artifacts including the download URL and credential credential bundles used. This command shows you Platform, App Version, Date, Type, Credentials and URL for a given Project and App. Since our Build Farm now supports native iOS, Android and Windows Phone 8, all of those are available too.


### fhc forms, fhc submissions and fhc themes


Our zero-code Drag and Drop Apps, based around Forms, had a massive upgrade in FeedHenry 3 and they have really been grabbing everyone's attention. Everything you can do with Forms in the Studio, you can also do on the command line. For Forms themselves, this includes list/create/update/get/delete. Similar commands are available for Forms Apps, Forms Groups, Forms Themes and Forms Notifications. Everything is built on JSON and can be easily modified and moved. All of the data submitted by users using Drag and Drop Apps is also available


### fhc templates


We have an ever expanding set of template/sample/tutorial Apps which can be listed and read on the command line.


### fhc ssh-keys and fhc user-keys


You can manage both your ssh keys and FeedHenry API Keys from the cli including commands like list/add/delete/read/update.


## Changes to existing commands




### fhc apps


The apps command, instead of just listing apps as it did in the past, contains all apps related sub commands like list/create/read/update/delete.

The following older apps related commands still exist but have been deprecated:



	
  * fhc read

	
  * fhc delete

	
  * fhc update

	
  * fhc delete




### fhc import


This is now invoked as follows:

    
    fhc import <project-id> <app-title> <app-template-type> [<zip-file> || <git-repo>]


Note: if no file or repo is specified, a bare git repo is created
