---
author: conor.oneill
categories:
- Developers
comments: false
date: 2016-02-16T14:24:54Z
link: http://www.feedhenry.com/get-started-with-the-red-hat-mobile-api-mapper/
slug: get-started-with-the-red-hat-mobile-api-mapper
title: Getting started with the Red Hat Mobile API Mapper
url: /get-started-with-the-red-hat-mobile-api-mapper/
wordpress_id: 5954
---

Two weeks ago [we announced the 3.7.0 release](http://www.feedhenry.com/release-of-rhmap-3-7/) of the Red Hat Mobile Application Platform including a Preview of our all new [API Mapper](http://docs.feedhenry.com/v3/guides/api_mapper.html). The API Mapper enables everyone from new Node.js developers to hardcore map-reducers to interact with existing back-end APIs and customise them for use in their mobile applications.

This simple-looking but extremely powerful tool provides the following features:




    
  * Create GET/POST/PUT/PATCH/DELETE/HEAD/OPTIONS requests

    
  * Set headers

    
  * Save your Requests

    
  * Create a Mapping from a Request

    
  * Map fields (Include/Exclude/Rename)

    
  * Carry out complex data Transforms using custom Node.js code

    
  * Mount mappings to specific end-points

    
  * Make mappings accessible to your Cloud Code

    
  * Automatically generate Node.js code and Curl requests



In this post, I'm going to walk through the functionality from the simplest request all the way to creating custom transforms.

There are a few simple setup steps needed in the Preview which you can read [in the Docs](http://docs.feedhenry.com/v3/guides/api_mapper.html) that I’m going to skip over. Note that you can use the API Mapper either within the Studio or popped out into its own browser tab.

[![apimapper02_small2](/wp-content/uploads/2016/02/apimapper02_small2.jpg)](/wp-content/uploads/2016/02/apimapper02_small2.jpg)





### Home Page



The home page shows you all of your existing saved Mappings. You can Edit, Delete or Export them as JSON files.



[![apimapper01_small2](/wp-content/uploads/2016/02/apimapper01_small2.jpg)](/wp-content/uploads/2016/02/apimapper01_small2.jpg)



### Enter a URL



To build a new mapping, you first start with a New Request.

[![apimapper03_small](/wp-content/uploads/2016/02/apimapper03_small.jpg)](/wp-content/uploads/2016/02/apimapper03_small.jpg)

Let’s use the OpenShift Online REST API to find out what version it is currently on and its status.

Their API is at:


    
    https://openshift.redhat.com/broker/rest/api





### Headers



Enter that URL and use the default GET option

The OpenShift API requires one simple header which is the type of response you want so add

Header Key = Accept
Header Value = application/json

[![apimapper04_small](/wp-content/uploads/2016/02/apimapper04_small.jpg)](/wp-content/uploads/2016/02/apimapper04_small.jpg)

and click SEND REQUEST.

You can click on the tabs to see the Raw Request, Response Headers and Raw Response.

[![apimapper05_small](/wp-content/uploads/2016/02/apimapper05_small.jpg)](/wp-content/uploads/2016/02/apimapper05_small.jpg)



[![apimapper06_small](/wp-content/uploads/2016/02/apimapper06_small.jpg)](/wp-content/uploads/2016/02/apimapper06_small.jpg)

You can continue to try out requests and add relevant headers (e.g for Auth) until you are satisfied with the basic request. Don’t forget you can use POST, PUT, DELETE, etc requests on your API if they are supported.



### Mount Path



Next you should give the request a reasonable Mount Path. Let’s use /osoapi

[![apimapper10_small](/wp-content/uploads/2016/02/apimapper10_small.jpg)](/wp-content/uploads/2016/02/apimapper10_small.jpg)

You can then click Create Request to save it in the Home Page list and start working with mappings and transformations.

Click Try Request to refresh the data.

Now we’re going to Map that API and turn it into something simple. So click Add Mapping.

[![apimapper07_small](/wp-content/uploads/2016/02/apimapper07_small.jpg)](/wp-content/uploads/2016/02/apimapper07_small.jpg)

You can now see all of the fields in a hierarchical collapse/expand view along with the Unmapped and Mapped output. At this point they are, of course, the same. Now start clicking on various elements and unticking them from “Use this field?”. Note that unticking a parent will untick all of the children too.

As you do this, the Mapped output is updated automatically. Untick everything except api_version and status and you will end up with this:

[![apimapper08_small](/wp-content/uploads/2016/02/apimapper08_small.jpg)](/wp-content/uploads/2016/02/apimapper08_small.jpg)



### Simple Transformations



The simplest and most common of all transformations is to rename so that is highlighted separately in the UI. Let’s rename api_version to current_version.

If you click on the Transform dropdown, the only option is “round” since it’s a numeric field. Another example is Arrays where you can pick “sum”. However let’s click on the status element in the api and then click on transform. As it’s text we can capitalize or lowercase. Let’s pick capitalize, which results in:

[![apimapper09_small](/wp-content/uploads/2016/02/apimapper09_small.jpg)](/wp-content/uploads/2016/02/apimapper09_small.jpg)



### Using the Mapped API



Now that we are happy with our Mapped API, we can use it in a variety of ways

First, you can see where it’s available directly on a URL by clicking the mount path.

[![apimapper11_small](/wp-content/uploads/2016/02/apimapper11_small.jpg)](/wp-content/uploads/2016/02/apimapper11_small.jpg)

And clicking on the Sample Code tab gives you everything you need to get going in your own coding. So you have




    
  * an fh.service snippet for use in your Cloud code

    
  * a direct Node.js request module snippet

    
  * and finally the correct Curl command



[![apimapper12_small](/wp-content/uploads/2016/02/apimapper12_small.jpg)](/wp-content/uploads/2016/02/apimapper12_small.jpg)



### Advanced Transformations using Node.js



For many users, the features I’ve just described are more than enough to work with. But for advanced users, we have one more card up our sleeve. You can write your own transformations in Node.js and make them available for use inside your instance of the API Mapper.

Let’s look at an example in the Studio Editor.

[![apimapper13_small](/wp-content/uploads/2016/02/apimapper13_small.jpg)](/wp-content/uploads/2016/02/apimapper13_small.jpg)

In application.js you can see a transformations object where you can specify the transformations you want to make available. In the case of our OpenShift API, it might be useful to return the minimum API version supported as a distinct thing. So let’s create a transformation called minimumArrayTransform which takes an array and returns the minimum value in it.

Update application.js like so:


    
    // fhlint-begin: custom-routes
     app.use('/', apiMapper({
     transformations: {
     // Add your custom transformations here! `customMixedArrayTransform` is an example of this.
     mixedArrayTransform: require('./transformations/mixedArrayTransform.js'),
     minimumArrayTransform: require('./transformations/minimumArrayTransform.js')
     }
     }));



[![apimapper14_small](/wp-content/uploads/2016/02/apimapper14_small.jpg)](/wp-content/uploads/2016/02/apimapper14_small.jpg)

And create a new minimumArrayTransform.js in the transformations directory with these contents:


    
    // First, tell the mapper what value types this transformation is valid for
     exports.type = "array";
     // Then, implement the transformation function
     exports.transform = function(arr){
     return arr.reduce(function (p, v) {
     console.log("p = " + p + " v = " + v)
     return ( p < v ? p : v );
     });
     };



[![apimapper15_small](/wp-content/uploads/2016/02/apimapper15_small.jpg)](/wp-content/uploads/2016/02/apimapper15_small.jpg)

Deploy your updated code and now go back to API Mapper and refresh the screen to pick up the new transform. Go to the Mapper section and re-enable the supported_api_versions field, change its name to minimum_api_version and select minimumArrayTransform from the drop-down list. You should now see the updated API output that you were looking for.

[![apimapper16_small](/wp-content/uploads/2016/02/apimapper16_small.jpg)](/wp-content/uploads/2016/02/apimapper16_small.jpg)



### Conclusion



You can expect to see lots more features coming in later releases and we’d be delighted to hear any feedback that you have.
