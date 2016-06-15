---
author: cian.clarke
categories:
- Developers
comments: false
date: 2012-09-26T10:54:52Z
link: http://www.feedhenry.com/feedhenry-workflow/
slug: feedhenry-workflow
title: FeedHenry Workflow
url: /feedhenry-workflow/
wordpress_id: 2818
---

If you've used the FeedHenry platform to develop mobile apps, chances are you've gotten to grips with some of our power user features. This post attempts to bring together all these features and tweaks that the Apps team here at FeedHenry use on a daily basis to build apps like a power user.


## FHC


The first tool of choice is our Command Line Interface (CLI). FHC allows you to list, create, build and deploy applications in FeedHenry from the command line. Why is this a power user feature? Most power users prefer to be able to perform tasks through a few keystrokes on a command line, rather than through a visual UI. Here are a few popular use cases for FHC:



	
  * A SCM system that doesn't support post receive hooks to automatically pull code after a checkin?

    
    fhc git pull someAppID




	
  * Find yourself frequently creating a series of iPhone and Android builds for distribution Over The Air (OTA)? Build a shell script that kicks these builds off through FHC, come back once done

	
  * Want to search through log output of an app in a live environment?

    
    fhc logs someAppID | grep -i 'ERROR: '




	
  * Need to quickly apply a hotfix?

    
    git push origin release
    fhc git pull someAppID
    fhc stage someAppID --live







### Aliases


To make the use of FHC more useful, we make use of aliases. The ID assigned to an app is really long - and while we can copy and paste, it's nice to be able to alias it to something else. Setting up an alias is simple:

    
    fhc alias myapp=sX7ITIjMxxLiZllZi0rRWHrM


Now, we can run commands like

    
    fhc logs myapp





### Working Locally


Another very useful power user feature of FHC is _fhc local_. Local allows developers to work on their app offline. To get set up for working locally, we first need to install dependencies, as you would with any Node.js application. To do this:

    
    cd cloud
    npm install -d
    cd ..


Now, we're good to go. We simply run

    
    fhc local


and open http://127.0.0.1:8888 (localhost) in our web browser. What this does in the background is runs the _main.js _cloud code from your cloud directory locally as a Node.js process, and also runs a webserver serving up the files in client/default/index.html.

There's many options available too, such as watching the local file system for changes, proxying requests through to the FeedHenry live environment rather than running the app locally and applying packages. To see all the options available, run

    
    fhc local --help





### Using Over The Air (OTA)


OTA makes installing builds on device much quicker. This feature is available for all Android developers, but only those in possession of an iOS Enterprise Development account ($299/yr rather than $99/yr) can sign apps Over The Air. To do this, you need to use a Distribution Certificate & Private Key, along with an In-House provisioning profile. Once these resources are uploaded to your account, you can use _fhc ota_ to generate OTA builds, and builds in the studio will also show an OTA install URl.


## Using Git


In a team-based environment, using some form of SCM is vital to the success of the project. We use git.

Usually, we'll have sprint branches, where the user stories for that sprint are checked in. All developers work off this sprint. Developers will develop and test code locally using _fhc local_ (described above), and once done commit it to the repository.
To make sure we don't need to _git pull_ our app every time, we use a post receive hook - this tells FeedHenry to grab the latest code on every commit.

Releases are done off either _master_ or, more than likely, a _release_ branch. The reason for using a release branch rather than master is it's too easy to accidentally check into _master_.
We have a corresponding FeedHenry studio app for 'dev' and 'release' - one of which points to the latest sprint branch, the other the release branch. When we're ready to ship new code, the sprint branch gets merged into release. We then stage the cloud code, and generate builds, both from the live project in FeedHenry.








## Screencast


Some of these points are illustrated in our screencast.







