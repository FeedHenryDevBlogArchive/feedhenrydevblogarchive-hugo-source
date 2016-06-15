---
author: david.martin
categories:
- Blog
- Developers
comments: false
date: 2014-02-21T16:45:55Z
link: http://www.feedhenry.com/server-side-pdf-generation-node-js/
slug: server-side-pdf-generation-node-js
tags:
- handlebars
- node
- node.js
- pdf
- phantomjs
title: Server side PDF generation with Node.js
url: /server-side-pdf-generation-node-js/
wordpress_id: 3506
---

# Overview of what was required


A solution in Node.js for dynamically generating a PDF version of submitted form data.
The form data is stored in a mongo database, backed by some mongoose models. What this boils down to is we have a collection (JSON array) of forms (JSON objects) with a number of key-value pairs. These key-value pairs have to be written out to a PDF file. The layout of the PDF file could be tweaked later to present the data in the best possible way. The tricky part was how to create the PDF file and add content to it.


# Options for creating PDFs




## PDFKit


The first option explored was PDFKit, 'a PDF generation library for Node.js' [http://pdfkit.org/](http://pdfkit.org/). The API looked good, but it was too high level. It would require a lot of time to get a dynamically generated layout working. This was an important factor as the order, types and number of fields to include in the PDF would be outside our control e.g. there could be any number of text fields followed by any number of image files, which could span multiple pages. Calculating where to put each of these would require a lot of time to get right, especially trying to prevent images spanning the bottom of 1 page and the top of the next page.

The next 2 options took a different approach of converting html to pdf. By working with html, it would allow us to get a nicer layout together much quicker. It also meant we could leverage existing experience with html & css.


## Wkhtmltopdf


wkhtmltopdf, [https://code.google.com/p/wkhtmltopdf/](https://code.google.com/p/wkhtmltopdf/), is a shell tool for converting html to pdf by using the webkit rendering engine. There is a Node.js module, by the same name, that wraps this shell tool [https://npmjs.org/package/wkhtmltopdf](https://npmjs.org/package/wkhtmltopdf). It looks like a great tool and provides a lot of features. However, we didn't choose this option because we were more familiar with the next option.


## PhantomJS


The third, and chosen option, was to use the built in PDF rendering capabilities of PhantomJS. PhantomJS is a headless WebKit with a javascript API. The API allows for writing acceptance tests, capturing screenshots or simply rendering and modifying html in a server-side environment. There are plenty of Node.js modules available to integrate with PhantomJS. We chose phantomjs-node [https://github.com/sgentle/phantomjs-node](https://github.com/sgentle/phantomjs-node). We're very familiar with PhantomJS, and use it for unit and functional tests for our frontend systems (via grunt [http://gruntjs.com/](http://gruntjs.com/)) and html5 Apps. This familiarily made the choice easier, but still left a few challenges.


# PhantomJS Solution




## Challenges


Rendering a PDF was the easiest part. Theres an API call [http://phantomjs.org/api/webpage/method/render.html](http://phantomjs.org/api/webpage/method/render.html). The generated PDF is saved to disk with the given filename.

    
    <code>page.render('/tmp/file.pdf', function() {
      // file is now written to disk
    });
    </code>


Some challenges to solve were:



	
  * how to control the size of the PDF i.e. have an A4 page size

	
  * how to dynamically build the html content

	
  * how to style the content in the PDF

	
  * how to load/inject images into the PDF




### Controlling the viewport size


There's a page API for setting the page size. The exact dimensions of the pages can be set, or the format can be specified e.g. A4.

    
    <code>page.set('paperSize', {
      format: 'A4'
    }, function() {
      // continue with page setup
    });
    </code>




### Dynamically building the PDF content


How to build the html dynamically was an easy decision. We went with a Handlebars template [http://handlebarsjs.com/](http://handlebarsjs.com/), and iterated over each of the form fields, constructing a different block of html depending on the field type. e.g.

    
    <code>{{#each fields}}
      {{#is 'file' type}}
        <a href="{{this.downloadUrl}}">{{this.downloadUrl}}</a><br/>
      {{/is}}
    
      {{#is 'number' 'radio' type}}
        <span>{{this}}</span>
      {{/is}}
    
      {{#is 'text' 'emailAddress' 'dropdown' type}}
        <p>{{this}}</p>
      {{/is}}
    {{/each}}
    </code>




### Styling the PDF content


Styling was done with CSS. Some specific styles were added to ensure images never overflow vertically (across pages) or horizontally (partially visible). An extra class was added to allow forcing a page break too.

    
    <code>* {
      overflow: visible !important; // forces a built-in PDF rendering gotcha in WebKit so that images never span 2 pages
    }
    img {
      margin-top:20px;
      max-width: 92%; // images will never overflow off the right hand side of a page
    }
    .page-break {
      display: block;
      page-break-before: always; // force a page break
    }
    </code>




### Loading/Injecting images into the PDF content


Images for the PDF are, like the rest of the content, private and stored behind an auth protected server. This meant that writing the src url of images as a direct image url wouldn't work. A Cookie could have been added to the PhantomJS session, but that posed other problems. Also, fetching remote images would have slowed down the rendering time.
To get around this problem, we retrieved any images needed for the PDF directly from gridfs, and saved them to the local filesystem. This allowed us to use the local filesystem image url e.g. file:///tmp/image.png.

    
    <code><!-- Load image from file:///, will have been pre-saved to disk -->
    <img src="{{this.localUrl}}"/><br/>
    </code>




## Extra Challenges


Adding this solution to a running Node.js server posed a couple of extra challenges:



	
  * how to download the PDF file

	
  * how to avoid the time overhead of initialising the PhantomJS process for every PDF render

	
  * how to avoid the PhantomJS process hanging around when the Node.js server stops




### Downloading the generated PDF in express


Our server that would generate these PDF's was a Node.js server using expressjs [http://expressjs.com/](http://expressjs.com/). Theres a handy one-liner in express to initiate a file download for a file on disk [http://expressjs.com/api.html#res.download](http://expressjs.com/api.html#res.download).

    
    <code>res.download('/file.pdf', 'file:///tmp/file.pdf');
    </code>




### Avoiding PhantomJS init for every PDF generation


The initial overhead of starting phantom was ~1-3 seconds. This meant a file download would take some time before the download actually began. To get around this, we only created a PhantomJS session if one was not already running, and keep a reference to it. Each PDF render would happen in a new page inside the same session. We had to ensure the page was closed when the rendering complete (or if the rendering failed). Creating a new page inside an existing PhantomJS session is a lot quicker than starting a new session.

    
    <code>var renderPdf = function(session, cb) {
      var page;
    
      try {
        session.createPage(function(_page) {
          page = _page;
          // ...
          var file = '/tmp/file.pdf';
          page.render(file, function() {
            page.close();
            page = null;
            return cb(null, file);
          });
        });
      } catch(e) {
        try {
          if (page != null) {
            page.close(); // try close the page in case it opened but never rendered a pdf due to other issues
          }
        } catch(e) {
          // ignore as page may not have been initialised
        }
        return cb('Exception rendering pdf:' + e.toString());
      }
    };
    </code>




### Avoiding PhantomJS processes hanging around


If the Node.js server was to stop (either intentially or a crash) and a PhantomJS session was open, the associated PhantomJS process would stay running. This would caused issues with extra resources being used and port conflicts when the server was restarted (PhantomJS listens on a few ports).
To prevent this from happening, we had to add a best effort attempt to shutdown the session when the Node.js server exits.

    
    <code>var session;
    var createPhantomSession = function(cb) {
      if (session) {
        return cb(null, session);
      } else {
        require('phantom').create({}, function(_session) {
          session = _session;
          return cb(null, session);
        });
      }
    };
    
    process.on('exit', function(code, signal) {
      session.exit();
    });
    </code>




## Performance


There are so many ways that this feature could have been implemented. The choices made above were driven by a few main factors:



	
  * Familiarity & previous experience with the tools & modules available

	
  * Research into available technologies

	
  * Personal choice

	
  * Performance implications of a paritcular solution


Some time was spent on performance testing during development. Various scenarios were run on the best way to start/stop a phantom session, and how best to use session pages, if at all. Each run generated a total of 100 PDFs. Results show observed time, cpu & memory for PhantomJS and Node.js for each scenario. The first run had the following critera:

	
  * create and teardown a phantomjs session for each page being rendered to PDF

	
  * load (1) image remotely from web server with an auth cookie

	
  * modify image source with jQuery after page is rendered (this wasnt needed in the end)

	
  * was not calling page.close after each page rendered (leaving processes hanging around)


<table >

<tr >
Comment
Time
Phantomjs Max CPU
Phantomjs Max Mem
Node Max CPU
Node Max Mem
</tr>

<tbody >
<tr >

<td >Create and teardown a phantom session each time
</td>

<td style="text-align: center;" >1m47.205s
</td>

<td style="text-align: right;" >n/a (too quick)
</td>

<td style="text-align: right;" >n/a (too quick)
</td>

<td style="text-align: right;" >2%
</td>

<td style="text-align: right;" >330M
</td>
</tr>
<tr >

<td >Create and leave a single phantom session open
</td>

<td style="text-align: center;" >0m19.954s
</td>

<td style="text-align: right;" >95%
</td>

<td style="text-align: right;" >338M
</td>

<td style="text-align: right;" >2%
</td>

<td style="text-align: right;" >130M
</td>
</tr>
<tr >

<td >Same as above,but calling page.close() after each gen
</td>

<td style="text-align: center;" >0m20.645s
</td>

<td style="text-align: right;" >95%
</td>

<td style="text-align: right;" >275M
</td>

<td style="text-align: right;" >2%
</td>

<td style="text-align: right;" >130M
</td>
</tr>
<tr >

<td >same as above,but all in parallel i.e. open 100 pages in phantomjs
</td>

<td style="text-align: center;" >0m22.139s
</td>

<td style="text-align: right;" >100%
</td>

<td style="text-align: right;" >700M
</td>

<td style="text-align: right;" >2%
</td>

<td style="text-align: right;" >117M
</td>
</tr>
<tr >

<td >same as above,but with jQuery loading/DOM manipulation removed
</td>

<td style="text-align: center;" >0m17.497s
</td>

<td style="text-align: right;" >n/a
</td>

<td style="text-align: right;" >n/a
</td>

<td style="text-align: right;" >n/a
</td>

<td style="text-align: right;" >n/a
</td>
</tr>
<tr >

<td >same as above,but with image loading using file:///
</td>

<td style="text-align: center;" >0m14.655s
</td>

<td style="text-align: right;" >n/a
</td>

<td style="text-align: right;" >n/a
</td>

<td style="text-align: right;" >n/a
</td>

<td style="text-align: right;" >n/a
</td>
</tr>
</tbody>
</table>
Although this was a lightweight test, it showed some useful information. The outcome of this can be summarised in the following notes (taken after an initial 'spike' and prior to the actual implementation):



	
  * creating a new phantom session for each pdf generation is slow, so best to keep a single phantomjs session alive and create pages as required.

	
  * need to ensure phantom session is closed on process exit (prevent phantomjs processes hanging about)

	
  * ensure the page is 'closed' each time after pdf generation to release memory [https://github.com/ariya/phantomjs/wiki/API-Reference-WebPage#wiki-webpage-close](https://github.com/ariya/phantomjs/wiki/API-Reference-WebPage#wiki-webpage-close)

	
  * avoid injecting external scripts if possible, jQuery in particular, as this can slow down the generation process

	
  * avoid evaluating code in the phantom page if possible as this causes extra requests over the phantom bridge [https://github.com/ariya/phantomjs/wiki/API-Reference-WebPage#evaluatefunction-arg1-arg2--object](https://github.com/ariya/phantomjs/wiki/API-Reference-WebPage#evaluatefunction-arg1-arg2--object). Better to include any required js in the initial content

	
  * avoid loading remote images as this adds time compared to loading files from disk (Overhead of retrieving binary from gridfs is less than retrieving image via http auth protected endpoint)

	
  * based on load tests, the best solution to avoid heavy cpu and memory usage is to not have any concurrency with pdf generation, and to use a queue. (However this recommendation was deferred due to expected load not being an issue currently)

	
  * A time limit could be imposed on pdf generation to avoid the queue hanging. Tests indicate that at best, a simple pdf can be generated every 150ms (~7/second). This time will vary depending on the number images/files and fields.




# Requirements Fulfilled


This solution uses a variety of Node.js modules, and PhantomJS, a cross OS tool that allows use to convert html to PDF.
An overview of some of the modules used:



	
  * phantomjs-node [https://github.com/sgentle/phantomjs-node](https://github.com/sgentle/phantomjs-node) (PhantomJS integration module for NodeJS)

	
  * Handlebars [https://github.com/wycats/handlebars.js/](https://github.com/wycats/handlebars.js/) (For dynamic templating)

	
  * Express [https://github.com/visionmedia/express](https://github.com/visionmedia/express) (Web framework)

	
  * Mongoose [http://mongoosejs.com/](http://mongoosejs.com/) (modelling for mongodb)


The outcome of this is a PDF generation feature where the content can be easily modified with html (and Handlebars templating), and the style can easily be modified with CSS.
