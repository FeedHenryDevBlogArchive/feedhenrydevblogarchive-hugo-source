---
author: brian.dooley
categories:
- Developers
comments: false
date: 2014-11-03T15:06:13Z
link: http://www.feedhenry.com/using-fh-db-in-your-apps/
slug: using-fh-db-in-your-apps
title: Using fh.db in your apps.
url: /using-fh-db-in-your-apps/
wordpress_id: 5160
---

# Introduction.



MongoDb is the leading NoSQL database available at the moment and the FeedHenry platform provides access to this hosted storage via the standard MongoDb driver or via the [FeedHenry _fh.db_ API](http://docs.feedhenry.com/v3/api/api_db.html).  Details of this API can be found in the _Docs_ section of your development studio but, for now, here is an example of how _fh.db_ can be used in your app to host a database that can be accessed by multiple apps.  We will ...




    
  * Create a single project.

    
  * Add a cloud app that acts as the interface to MongoDb using fh.db.

    
  * Add a 2nd cloud app that calls this apps endpoints.

    
  * Add a client app that will act as the user interface.





# 





# Overview.



[![fhdb shrunk](/wp-content/uploads/2014/10/fhdb-shrunk.png)](/wp-content/uploads/2014/10/fhdb-shrunk.png)



The project consists of 3 apps that reside in 3 public GitHub repositories.

<table width="534" style="height: 101px; width: 534px;" border="1px" class=" alignleft" >
<tbody >
<tr >

<td >**Name**
</td>

<td >**URL**
</td>
</tr>
<tr >

<td >fhdb_cloud_main
</td>

<td >[https://github.com/FeedHenrySuprt/fhdb_cloud_main](https://github.com/FeedHenrySuprt/fhdb_cloud_main)
</td>
</tr>
<tr >

<td >fhdb_cloud_app
</td>

<td >[https://github.com/FeedHenrySuprt/fhdb_cloud_app](https://github.com/FeedHenrySuprt/fhdb_cloud_app)
</td>
</tr>
<tr >

<td >fhdb_client
</td>

<td >[https://github.com/FeedHenrySuprt/fhdb_client_app](https://github.com/FeedHenrySuprt/fhdb_client_app)
</td>
</tr>
</tbody>
</table>











## Part 1 - Creating the project.



The code for this app is stored in GitHub.  Here is how to use it.




    
  * In your FeedHenry (V3) studio, create a "bare" project.  You can find instructions on getting started on this in the _Getting Started_ section of the docs.




    
  * Once created, add a new cloud app to the project by clicking the "+" under the _Cloud Code Apps_ section on the project view.




    
  * Choose _Import Existing App_ when given the option, click _Next_




    
  * Give your app a name.  For this example, we will name it _fhdb_cloud_main_.  Click _Next_.

    
  * Select _Public Git Repository_ and enter [https://github.com/FeedHenrySuprt/fhdb_cloud_main.git](https://github.com/FeedHenrySuprt/fhdb_cloud_main.git) as the URL.  Make sure that you put _master_ as the branch.




    
  * Hit _Import & Move onto Integration_ to create your app.



That's the 1st app done.  Now select _Apps, Cloud Apps & Services_ to go to the project overview. Create a 2nd cloud app by clicking the "+" under the _Cloud Code Apps_ section.  The procedure is the same as for the _fhdb_cloud_main_ app but use [https://github.com/FeedHenrySuprt/fhdb_cloud_app.git](https://github.com/FeedHenrySuprt/fhdb_cloud_app.git) as the URL and _master_ as the branch.  For the purposes of this example, we will call it _fhdb_cloud_app._ Finally, for this section, create a client facing app.  This is the same procedure again, except for the following ...




    
  * Add the app by pressing the "+" under the _Apps_ section of the project view.

    
  * Choose the _Cordova Light_ option when presented.  Use [https://github.com/FeedHenrySuprt/fhdb_client_app.git](https://github.com/FeedHenrySuprt/fhdb_client_app.git) as the URL and _master_ as the branch.

    
  * We shall call this app _fhdb_app_.

    
  * Follow the instructions that are presented for integrating the app into FeedHenry.



Once this is done, you should have 2 cloud apps and 1 client facing app.



## 





## Part 2 - Adjustments.



The _fhdb_cloud_app _app we have created needs to know the cloud url of the first app in order to communicate with it.  We can ensure this by placing the value of the URL in an environmental variable which is available to _fhdb_cloud_app_.




    
  * Go to the _Details_ section of the _fhdb_cloud_main_ app and copy the value of _Current Host_.

    
  * In the _fhdb_cloud_app_, go to _Environmental Variables, _create an APP_URL variable and give it the value of _Currant Host_ that you have just copied.







## Part 3 - Using the app.



In the _fhdb_app_, you can perform _fh,db_ functions in the preview.  Just make sure that your app is pointing to the _fhdb_cloud_app _in the preview.



[![Screenshot 2014-10-16 16.56.58](/wp-content/uploads/2014/10/Screenshot-2014-10-16-16.56.58.png)](/wp-content/uploads/2014/10/Screenshot-2014-10-16-16.56.58.png)























































You can ...




    
  * Add entries to the collection by pressing _Add Mr Jones to MongoDb_ or _Add Mr Smith to MongoDb_.

    
  * Clear the database by selecting _Delete From MongoDb_.

    
  * List contents of the database by pressing _List From MongoDb_.

    
  * List "Mr Smith" entries by pressing _List Mr Smith Entries_.



Results from each action will appear in the pale green section of the app.



## Summary.



As you can see from examining the code of these 3 apps ...




    
  * We have one app, _fhdb_cloud_main,_ which maintains an instance of MongoDb and exposes endpoints to perform actions on and return data from this instance.

    
  * The 2nd, _fhdb_cloud_app_, app is an example of a 2nd cloud app that calls these endpoints

    
  * The 3rd app, running on a device, performs _fh.cloud_ calls to its own cloud app, _fhdb_cloud_app_, and displays the results on screen.


