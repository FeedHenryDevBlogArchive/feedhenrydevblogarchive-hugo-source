---
author: cian.clarke
categories:
- Blog
- Developers
comments: false
date: 2014-12-08T15:36:19Z
link: http://www.feedhenry.com/web-scraping-for-fun-profit-ii/
slug: web-scraping-for-fun-profit-ii
title: Web Scraping for Fun & Profit II
url: /web-scraping-for-fun-profit-ii/
wordpress_id: 5180
---

In my [Web Scraping blog post](http://www.feedhenry.com/web-scraping-fun-profit/), I described some simple ways to retrieve data from a web page using Node.js. This means of data retrieval proves useful when no API is available.
Today, I'm going to describe similar techniques where reverse engineering & more complex authentication flows are involved to retrieve data.





## Authentication





When we need to authenticate before viewing the data we need to scrape, the complexity increases significantly. There's many different types of authentication - the vast majority of which we can tackle in Node.js.





When an authentication API absolutely can't be exposed, for whatever reason, my next point of call is to the web version of the login. We're going to reverse engineer the login flow of my demo FeedHenry platform instance, and doing this requires as much information about the login flow as possible.
First, it's over to the web browser to see how things work on the website. Using the Google Chrome inspector, and turning on recording of requests across page loads, we can see how the login flow works. Note I've also ticked the 'Preserve Log' option.





[![Screen Shot 2014-12-01 at 16.14.27](/wp-content/uploads/2014/12/Screen-Shot-2014-12-01-at-16.14.27-300x99.png)](/wp-content/uploads/2014/12/Screen-Shot-2014-12-01-at-16.14.27.png)





After a login request has been sent, we can inspect the request body, and find the information we need to reconstruct this request in Node.





[![](/wp-content/uploads/2014/12/login2-300x243.png)](/wp-content/uploads/2014/12/login2.png)






    
  1. [Request URL](https://gist.github.com/cianclarke/beb3ee79e0ebfed62f0b#file-feedhenrylogin-js-L8)

    
  2. [Request Method](https://gist.github.com/cianclarke/beb3ee79e0ebfed62f0b#file-feedhenrylogin-js-L9)

    
  3. We note the response headers contains a cookie which enables us to authenticate future requests

    
  4. We take note of the content type - in this case, `application/json` data. (Passing[ a 'json' property](https://gist.github.com/cianclarke/beb3ee79e0ebfed62f0b#file-feedhenrylogin-js-L11-L14) to request automatically gives us this content type).

    
  5. [We see the JSON payload being transmitted. ](https://gist.github.com/cianclarke/beb3ee79e0ebfed62f0b#file-feedhenrylogin-js-L11-L14)



This is a pretty simple example, but sometimes more advanced params are required. A good way to debug is right click on the request in the inspector tab (highlighted in blue above), and click "Copy as cURL". This will copy a cURL command to your clipboard which you can run in a terminal window. Start dropping headers and other pieces of information that look unimportant until you're left with a bare-bones working request.

From gathering this info in the chrome inspector, we can construct a [request in Node replicating this login flow](https://gist.github.com/cianclarke/beb3ee79e0ebfed62f0b#file-feedhenrylogin-js):


    
    var request = require('request'),
    jsdom = require('jsdom'),
    jar = request.jar(), // stores cookies in the "jar", we can re-use them in future authenticated requests
    
    // A closure function to enclose our code within
    (function(username, password){
      request.post({
        url : 'https://testing.feedhenry.me/box/srv/1.1/act/sys/auth/login',
        method : 'post',
        jar : jar,
        json : {
          u : username,
          p : password,
          d : "testing"
        },
      }, function(err, res, body){
        if (err){
          console.error(err);
          return;
        }
    
        // We're in - we can now do subsequent, authenticated requests within the website by just including the same cookie jar, like so:
        
        request.get({
          url : 'https://testing.feedhenry.me/box/api/projects',
          jar : jar,
          json : true
        }, function(err, res, body){
          console.log('Got ' + body.length + ' projects');
        });
        
      });
    })("blog@demo.com", "Blogpost123");
    



If we succeed, because our login information is stored in the shared "Cookie Jar" [we constructed at the start](https://gist.github.com/cianclarke/beb3ee79e0ebfed62f0b#file-feedhenrylogin-js-L3), we can now make authenticated requests to [other parts of the system](https://gist.github.com/cianclarke/beb3ee79e0ebfed62f0b#file-feedhenrylogin-js-L23-L30).
This is a very simple way of performing a login flow.



## Interval-based Scraping: Cron & Cache



Sometimes, we want to be able to reduce the time it takes for our scraper to run. Usually, we'll do this on an interval by caching the data.

To do this, we'll [extract our previous scraper code into a function](https://gist.github.com/cianclarke/4e78314aafcc1b41ca44#file-polling-js-L10). Then, we'll modify our previous scraper - instead of `console.log`-ing the number of projects, we'll [put them into a simple in-memory cache](https://gist.github.com/cianclarke/4e78314aafcc1b41ca44#file-polling-js-L33) using the [memory-cache module](https://github.com/ptarjan/node-cache).
Now, we [call our scraper function once](https://gist.github.com/cianclarke/4e78314aafcc1b41ca44#file-polling-js-L39) to prime the cache, and [set up an interval](https://gist.github.com/cianclarke/4e78314aafcc1b41ca44#file-polling-js-L40-L42) (every [15 mins](https://gist.github.com/cianclarke/4e78314aafcc1b41ca44#file-polling-js-L8) in this case) to refresh the cache.
Lastly, we use a [simple web server to respond with the cached data](https://gist.github.com/cianclarke/4e78314aafcc1b41ca44#file-polling-js-L46-L50).

This could easily be expanded using the fantastic [`node-cron` module ](https://github.com/ncb000gt/node-cron)to allow us to set up more advanced schedules in a unix crontab fashion. It'd also be advisable to use Redis for caching this data over an in-memory cache - just use [$fh.cache](http://docs.feedhenry.com/v3/api/api_cache.html) in FeedHenry.



## Other Popular Authentication Mechanisms



A myriad of authentication mechanisms exist- often much more advanced use cases. I've attempted to cover some here, with a worked example.



### **Form Data**



We can identify Form Data in the browser by looking for the request content-type `application/x-www-form-urlencoded`, along with a Form Data section in the request inspector of the browser:

[![](/wp-content/uploads/2014/12/Screen-Shot-2014-12-01-at-16.39.00-300x62.png)](/wp-content/uploads/2014/12/Screen-Shot-2014-12-01-at-16.39.00.png)

Node.js makes Form Data really simple. It can be appended to the request by replacing our [`json` option with `form`.](https://gist.github.com/cianclarke/21aacdd06f22d33e663c#file-gistfile1-js-L10-L14)



### **CSRF Tokens**



Some sites will require a [CSRF (Cross Site Request Forgery)](http://en.wikipedia.org/wiki/Cross-site_request_forgery) token from the login page before even performing the login request. This token is usually a hidden input on the page. If we can identify the CSRF token in the browser, we can then search the HTML source of the page for it.
To overcome these tokens in node.js code, we can first load the login page using a `GET` request, and then use JSDom to pick out the CSRF token.



### **Redirects**



Another popular flow is many redirects before reaching final destination - [the 'followAllRedirects' option of request](https://github.com/mikeal/request#requestoptions-callback) is useful when this is required.



### An Example - OpenShift Online



The login form for OpenShift Online illustrates these concepts perfectly - namely:




    
  1. A CSRF-like token [pulled from the page using JSDom](https://gist.github.com/cianclarke/54d58cad0c3a9cbecdd5#file-gistfile1-txt-L17)

    
  2. [Form data](https://gist.github.com/cianclarke/54d58cad0c3a9cbecdd5#file-gistfile1-txt-L24-L30)

    
  3. [The need to follow through redirects](https://gist.github.com/cianclarke/54d58cad0c3a9cbecdd5#file-gistfile1-txt-L9)




    
    var request = require('request'),
    jsdom = require('jsdom'),
    jar = request.jar(); // stores cookies in the "jar", we can re-use them in future authenticated requests
    
    request({
      url : 'https://openshift.redhat.com/app/login',
      method : 'get',
      jar : jar,
      followAllRedirects : true
    }, function(err, response, body){
      if (err){
        console.error(err);
        return;
      }
      jsdom.env(body,["http://code.jquery.com/jquery.js"], function (errors, window) {
        var $ = window.$,
        token = $('input[name=authenticity_token]').val(),
        utf8 = $('input[name=utf8]').val();
        request.post({
          url : 'https://openshift.redhat.com/app/login',
          method : 'post',
          jar : jar,
          followAllRedirects : true,
          form : {
            "web_user[rhlogin]" : "foo@bar.com",
            "web_user[password]" : "foobar",
            "commit" : "Sign in",
            "authenticity_token" : token,
            "utf8" : utf8
          }
        }, function(err, res, body){
          if (body.indexOf('My Account')>-1){
            console.log('We\'re in, and have headers in the jar to allow us to do authenticated requests!');
          }
        });
      });
    });
    





### **Token based Authentication**



Many authentication systems can perform authentication flows where a username and password is passed to the backend, and it returns an authentication token which is appended to future request bodies. This is common with SOAP based services, but can exist in JSON APIs too. How these work are best investigated using the network tab of the inspector, then mimicking the flow in code.



### **Debugging with a Proxy**



[![Using Charles Proxy](/wp-content/uploads/2014/12/Screen-Shot-2014-12-05-at-09.58.33-300x196.png)](/wp-content/uploads/2014/12/Screen-Shot-2014-12-05-at-09.58.33.png)If we're trying to replicate the login flow of a mobile app, command line utility, or software tool that doesn't live in a browser, I often use the Charles debugging proxy to inspect the data flow just like we did with the browser above. To use this, your target needs to support a HTTP proxy. Most command line utilities and software packages do, and to set it up with a mobile phone, just use the system proxy. [See my previous post on this](http://cianclarke.com/blog/how-popular-apps-access-apis/) for more details on using a debugging proxy.
It's also beneficial to be able to track the progress of requests through a Node.js application - you can use the charles proxy when using request like so:


    
    request.get({
      url : 'http://www.google.ie',
      proxy : 'http://127.0.0.1:8080'
    }, function(error, response, body){ 
      // This request has now flown through the Charles proxy
    });
    





# Further Reading & Tools






    
  * [Web Scraping for Fun & Profit I](http://www.feedhenry.com/web-scraping-fun-profit/) - Precursor to this post

    
  * [JSDom](https://github.com/tmpvar/jsdom) - Run jQuery DOM queries (& others) on a web page from ServerSide

    
  * [PhantomJS](http://phantomjs.org/) - Headless browser & JavaScript API

    
  * [Charles](http://www.charlesproxy.com/) -  Debugging Proxy

    
  * [CasperJS](http://casperjs.org/) - Testing utility which could also be used to build advanced scraping utilities




