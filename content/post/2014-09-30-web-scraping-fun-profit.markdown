---
author: cian.clarke
categories:
- Blog
- Developers
comments: false
date: 2014-09-30T19:15:58Z
link: http://www.feedhenry.com/web-scraping-fun-profit/
slug: web-scraping-fun-profit
tags:
- HTML
- JSDom
- legacy APIs
- MBaaS
- Mobile Integration
- node.js
- screen scraping
- web development
title: Web Scraping for Fun & Profit
url: /web-scraping-fun-profit/
wordpress_id: 5008
---

There’s a number of ways to retrieve data from a backend system within mobile projects. In an ideal world, everything would have a RESTful JSON API - but often, this isn’t the case.Sometimes, SOAP is the language of the backend. Sometimes, it’s some proprietary protocol which might not even be HTTP-based. Then, there’s scraping.





Retrieving information from web sites as a human is easy. The page communicates information using stylistic elements like headings, tables and lists - this is the communication protocol of the web. Machines retrieve information with a focus on structure rather than style, typically using communication protocols like XML or JSON. Web scraping attempts to bridge this human protocol into a machine-readable format like JSON. This is what we try to achieve with web scraping.





As a means of getting to data, it don’t get much worse than web scraping. Scrapers were often built with Regular Expressions to retrieve the data from the page. Difficult to craft, impossible to maintain, this means of retrieval was far from ideal. The risks are many - even the slightest layout change on a web page can upset scraper code, and break the entire integration. It’s a fragile means for building integrations, but sometimes it’s the only way.





Having built a scraper service recently, the most interesting observation for me is how far we’ve come from these “dark days”. Node.js, and the massive ecosystem of community built modules has done much to change how these scraper services are built.





## Effectively Scraping Information





Websites are built on the Document Object Model, or DOM. This is a tree structure, which represents the information on a page.By interpreting the source of a website as a DOM, we can retrieve information much more reliably than using methods like regular expression matching. The most popular method of querying the DOM is using jQuery, which enables us to build powerful and maintainable queries for information. The [JSDom](https://github.com/tmpvar/jsdom) Node module allows us to use a DOM-like structure in serverside code.





For purpose of Illustration, we're going to scrape the blog page of FeedHenry's website. [I’ve built a small code snippet](https://gist.github.com/cianclarke/78b67dd614403cd90fb8) that retrieves the contents of the blog, and translates it into a JSON API. To find the queries I need to run, first I need to look at the HTML of the page. To do this, in Chrome, I right-click the element I'm looking to inspect on the page, and click "Inspect Element".



[caption id="attachment_5025" align="alignnone" width="631"][![Screen Shot 2014-09-30 at 10.44.38](/wp-content/uploads/2014/09/Screen-Shot-2014-09-30-at-10.44.38.png)](/wp-content/uploads/2014/09/Screen-Shot-2014-09-30-at-10.44.38.png) Articles on the FeedHenry blog are a series of 'div' elements with the '.itemContainer' class[/caption]



Searching for a pattern in the HTML to query all blog post elements, we construct the `div.itemContainer` query. In jQuery, we can iterate over these using the .each method:




    
    var posts = [];
    $('div.itemContainer').each(function(index, item){
      // Make JSON objects of every post in here, pushing to the posts[] array
    });





From there, we pick off the heading, author and post summary using a child selector on the original post, querying the relevant semantic elements:





[![](/wp-content/uploads/2014/09/scrapingChildElements.png)](/wp-content/uploads/2014/09/scrapingChildElements.png)






    
  1. Post Title, using jQuery:

    
    $(item).find('h3').text()trim() // trim, because titles have white space either side




    
  2. Post Author, using jQuery:

    
    $(item).find('.catItemAuthor a').text()




    
  3. Post Body, using jQuery:

    
    $(item).find('p').text()








Adding some JSDom magic to our snippet, and pulling together the above two concept (iterating through posts, and picking off info from each post), we get [this snippet](https://gist.github.com/cianclarke/78b67dd614403cd90fb8):




    
    var request = require('request'),
    jsdom = require('jsdom');
    
    jsdom.env(
      "http://www.feedhenry.com/category/blog",
      ["http://code.jquery.com/jquery.js"],
      function (errors, window) {
        var $ = window.$, // Alias jQUery
        posts = [];
        $('div.itemContainer').each(function(index, item){
          item = $(item); // make queryable in JQ
          posts.push({
            heading : item.find('h3').text().trim(),
            author : item.find('.catItemAuthor a').text(),
            teaser : item.find('p').text()
          });
        });
        console.log(posts);
      }
    );
    



[![](/wp-content/uploads/2014/09/Screen-Shot-2014-09-30-at-11.03.52-300x91.png)](/wp-content/uploads/2014/09/Screen-Shot-2014-09-30-at-11.03.52.png)



### A note on building CSS Queries



As with styling web sites with CSS, building effective CSS queries is equally as important when building a scraper. It's important to build queries that are not too specific, or likely to break when the structure of the page changes. Equally important is to pick a query that is not too general, and likely to select extra data from the page you don't want to retrieve.

A neat trick for generating the relevant selector statement is to use Chrome's "CSS Path" feature in the inspector. After finding the element in the inspector panel, right click, and select "Copy CSS Path". This method is good for individual items, but for picking repeating patterns (like blog posts), this doesn't work though. Often, the path it gives is much too specific, making for a fragile binding. Any changes to the page's structure will break the query.



## Making a Re-usable Scraping Service



Now that we've retrieved information from a web page, and made some JSON, let's build a reusable API from this. We're going to make a FeedHenry Blog Scraper service in FeedHenry3. For those of you not familiar with service creation, see [this video walkthrough.](http://www.feedhenry.com/creating-re-usable-mbaas-services-feedhenry-3/)

We're going to start by creating a "new mBaaS Service", rather than selecting one of the off-the-shelf services. To do this, we modify the `application.js` file of our service to include one route, `/blog`, which includes our code snippet from earlier:


    
    // just boilerplate scraper setup
    var mbaasApi = require('fh-mbaas-api'),
    express = require('express'),
    mbaasExpress = mbaasApi.mbaasExpress(),
    cors = require('cors'),
    request = require('request'),
    jsdom = require('jsdom');
    
    var app = express();
    app.use(cors());
    app.use('/sys', mbaasExpress.sys([]));
    app.use('/mbaas', mbaasExpress.mbaas);
    app.use(mbaasExpress.fhmiddleware());
    // Our /blog scraper route
    app.get('/blog', function(req, res, next){
      jsdom.env(
        "http://www.feedhenry.com/category/blog",
        ["http://code.jquery.com/jquery.js"],
        function (errors, window) {
          var $ = window.$, // Alias jQUery
          posts = [];
          $('div.itemContainer').each(function(index, item){
            item = $(item); // make queryable in JQ
            posts.push({
              heading : item.find('h3').text().trim(),
              author : item.find('.catItemAuthor a').text(),
              teaser : item.find('p').text()
            });
          });
          return res.json(posts);
        }
      );
    });
    app.use(mbaasExpress.errorHandler());
    
    var port = process.env.FH_PORT || process.env.VCAP_APP_PORT || 8001;
    var server = app.listen(port, function() {});
    



We're also going to write some [documentation for our service](http://docs.feedhenry.com/v3/product_features/services.html#services-service_documentation), so we (and other developers) can interact with it using the FeedHenry discovery console. We're going to modify the `README.md` file to document what we've just done using [API Blueprint](http://apiblueprint.org/) documentation format:


    
    # FeedHenry Blog Web Scraper
    This is a feedhenry blog scraper service. It uses the `JSDom` and `request` modules to retrieve the contents of the FeedHenry developer blog, and parse the content using jQuery.
    # Group Scraper API Group
    # blog [/blog]
    Blog Endpoint
    ## blog [GET]
    Get blog posts endpoint, returns JSON data.
    + Response 200 (application/json)
        + Body
                [{ blog post}, { blog post}, { blog post}]
    



We can now try out the scraper service in the studio, and see the response:

[![](/wp-content/uploads/2014/09/Screen-Shot-2014-09-30-at-15.03.10-300x252.png)](/wp-content/uploads/2014/09/Screen-Shot-2014-09-30-at-15.03.10.png)



## Scraping - The Ultimate in API Creation?



Now that I've described some modern techniques for effectively scraping data from web sites, it's time for some major caveats. First,  WordPress blogs like ours already have feeds and APIs available to developers - there's no need to ever scrape any of this content. Web Scraping is **not a replacement for an API**. It should be used only as a **last resort**, after every endeavour to discover an API has already been made. Using a web scraper in a commercial setting requires much time set aside to maintain the queries, and an agreement with the source data is being scraped on to alert developers in the event the page changes structure.
With all this in mind, it can be a useful tool to iterate quickly on an integration when waiting for an API, or as a fun hack project.

_[@cianclarke](http://www.twitter.com/cianclarke)[ ](/wp-content/uploads/2014/06/Screen-Shot-2014-06-11-at-15.56.07.png)is a Software Engineer with FeedHenry. Primarily focused on the mobile-backend-as-a-service space, Cian is responsible for many of FeedHenry's mBaaS developer features._
  *[mBaaS]: Mobile Backend As A Service
