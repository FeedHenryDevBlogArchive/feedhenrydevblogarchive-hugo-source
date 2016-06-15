---
author: edewit
categories:
- Blog
- Developers
comments: false
date: 2015-06-10T13:29:44Z
link: http://www.feedhenry.com/aerogear-push-templates/
slug: aerogear-push-templates
tags:
- aerogear
- mobile application platform
- red hat
title: AeroGear quick start push Templates
url: /aerogear-push-templates/
wordpress_id: 5501
---

Currently in software development the biggest challenge is integrating. A lot of what we do involves integrating with other platforms or libraries. Knowing how to integrate is important, so here I'm going to show how to integrate the Unified Push Server (UPS) with Feedhenry, for adding push notification support. To make things even easier we created a Feedhenry Template that you can use as a starting point for your application that contains all the parts you'll need.

To illustrate how to use the FeedHenry Template for push notifications, you will build a set of sample applications for receiving sports news. You will use a centralized cloud application that integrates with both a push server as well as any client applications. The centralized cloud app contains various categories (i.e. different sports) and news stories associated with one or more categories. There are also two client applications:




    
  * Console web application: Let administrators create and update the category list, as well as create news stories on the centralized cloud app

    
  * Mobile application: Lets users subscribe to any of the available categories and receive a push notification whenever a new story is posted with that category



To build a project based on this template:


    
  1. Create a new project.

    
  2. Click **Sample Projects**, and then find the Project named: **Push Hello World**.

    
  3. Click select type a name, and then click **create**.



![1](/wp-content/uploads/2015/05/1.png)



Now you have three apps one Cloud Code App and two Client Apps.

[![5](/wp-content/uploads/2015/05/5.png)](/wp-content/uploads/2015/05/5.png)
The _Push Cloud App_ is a simple server-side CRUD application for categories. The _Push Console App_ is a "management inteface" where you create and delete these categories. These categories are used by the _Push Notifications Mobile Client_ to "subscribe" to. For example I have a sports news site and I push messages about different sports to the mobile users. The users _subscribe_ to different sports they want to receive notifications about.

With this generated project all that is left to do is connect the Unified Push Server to this project, first go to openshift.com and create an UPS instance, search on AeroGear pick a name and "Create Application". For more information on how to set up a unified push server on Openshift have a look at the [AeroGear documentation](https://aerogear.org/docs/unifiedpush/ups_userguide/index/#openshift). Now you have a running UPS on Openshift you need to login and create an "application" that will be used by your Feedhenry apps to send push notifications. Note down the `Application ID`, `Master Secret` and the `Server URL` as we going to need those later on.

[![10](/wp-content/uploads/2015/05/10.png)](/wp-content/uploads/2015/05/10.png)

For the clients that we want to use (e.g. Android iOS) we need to create variants, for more information on how to do this have a look at the UPS documentation for [Android](https://aerogear.org/docs/unifiedpush/aerogear-push-android/) and [iOS](https://aerogear.org/docs/unifiedpush/aerogear-push-ios/)

Next we need to do two things we need create a "Service connector" for UPS and we need to tell the Cloud App the `GUID` of the "Service connector" so that it can use this to send messages to UPS. First we setup the "Service connector" to connect to our openshift UPS instance, go to the `Services &amp; APIs` tab and click `Provision mBaaS Service/API`:

[![3](/wp-content/uploads/2015/05/3.png)](/wp-content/uploads/2015/05/3.png)
Then choose `AeroGear Push Service`, give it a name and click `Next`, fill in the `Server URL`, `Application ID` and `Master Secret` and click `Next` now it will create your "Service connector"

[![7](/wp-content/uploads/2015/05/7.png)](/wp-content/uploads/2015/05/7.png)
After that is done go back to your project, here we need to add the service so that we can use it click the "+" in the mBaas Services box in the overview page:

[![2](/wp-content/uploads/2015/05/2.png)](/wp-content/uploads/2015/05/2.png)
Select the created "Service connector" and then the "Associate Service" button. Now you'll see the "Service Connector" in the "mBaas Services" column. Let's configure the "Cloud App" to use this "Service Connector", for that we need to the GUID of the "Service Connector", click on it and you will see the info page:

[![4](/wp-content/uploads/2015/05/4.png)](/wp-content/uploads/2015/05/4.png)

On the info page copy the `Service ID` and under access control add you project, then click save.

Then in the project click on the "Push Cloud App" and select "Environment variables" from the sidebar. Click "Add Variable" and enter `AEROGEAR_SERVICE_GUID` for name and paste the `Service ID` you copied earlier for the "Development" label. Next click on "Push Environment Variables" to publish these.

[![6](/wp-content/uploads/2015/05/6.png)](/wp-content/uploads/2015/05/6.png)
Now that that is all setup, the "Cloud App" can use the "GUID" to fetch the "Service Connector", that connects to UPS to send the messages.

Let's fire up a browser and open the _Push Console App_ to create a couple of categories for instance: Football, Rugby and Basketball:

[![8](/wp-content/uploads/2015/05/8.png)](/wp-content/uploads/2015/05/8.png)

Now go to your _Push Notifications Mobile Client_ and open the file called `push-config.json`. You'll need to update the _Variant ID_ and _Variant Secret_ for each platform and the _pushServerURL_ to the location of your UnifiedPush Server, so that it can register itself with the UnifiedPush Server in order to receive messages. For Android you also need to specify the _senderID_ (also called project number on the google console) inside of that JSON file. Finally build and install the _Push Notifications Mobile Client_ on a device and select the categories you want to receive.

Now go back to the _Push Console App_ and send a message. If all worked well, you will see how the push notifications arrive on the mobile client.

[![9](/wp-content/uploads/2015/05/9.png)](/wp-content/uploads/2015/05/9.png)
You can use and extend this template for your own use case. More detailed information on the setup of this template can be found in the help of the App Console
