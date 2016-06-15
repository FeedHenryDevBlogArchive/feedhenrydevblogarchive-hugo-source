---
author: cian.clarke
categories:
- Developers
comments: false
date: 2012-07-25T11:00:25Z
link: http://www.feedhenry.com/building-a-developer-studio-in-nodejs/
slug: building-a-developer-studio-in-nodejs
title: Building a Developer Studio in NodeJS
url: /building-a-developer-studio-in-nodejs/
wordpress_id: 2826
---

Back in December of 2011, our CTO Mícheál had an interesting concept: What if we tried building a developer studio on top of our Node.js Module, FHC? Here's how we went about doing just that.

Some background: Since November 2011, FeedHenry has had a Node.js module that interfaces with the FeedHenry platform. It's primary purpose was as a CLI, but since it was written in Node.js, it was perfect to use as a gateway into FeedHenry to do other stuff! FHC was originally an internal tool, but we found it such a great tool to our workflows that we decided to open source the module, and open it up to the world. Since then, we've been building this tool in the open, adding functionality as people using the platform demand. It served as the perfect basis for building a studio.

The fh-skunkworks department spent a few long nights hacking, the output of which was a simple architecture forming the basis of this project: ExpressJS, FHC, Node.js and Bootstrap. The first iteration did little more than show a list of apps in a user's account in a very shiny matter. The finished project was a different beast entirely. What follows is a look at some of the cool stuff we learned along the way.


## Express as a Framework


Express has been around since the earliest days of Node. It gives support for routing, templating, static directory support & loads more out of the box. It achieves lots of it's core functionality by extending Sencha's Connect module for Node middleware.
It powers all the route handling of the studio's API, generates on-the-fly stylesheets, and renders views.


## Templating Engines


The project started with using EJS as it's templating engine. Based on JavaScript, the syntax is immediately apparent, and Express supports EJS out of the box.
Some research revealed that EJS wasn't the only contender - a whole host of templating languages exist. There's Jade, HAML, Mustache, and of course Dust. LinkedIn had some [interesting musings](http://engineering.linkedin.com/frontend/leaving-jsps-dust-moving-linkedin-dustjs-client-side-templates) on the subject, and their evaluation lead us to chose Dust.
Dust promotes logic-less templates, with any logic pushed out to helper functions. In theory, this sounds great.
The reality, I've found, was this was too lofty an ambition. It's incredibly useful to have short one-line conditional wrapping an element. This proves particularly difficult in Dust, and is a breeze in EJS.
Another issue with Dust is the project is no longer actively maintained, and doesn't work with Node > 0.6.*, meaning we have to point to a branch of the project we maintain.
There are two distinct schools of thought when it comes to templating - logic, or logicless. Dust falls into the latter, and whilst I'm sure it works for some projects, I found it lead to unwieldy and convoluted helper functions which could have been easier achieved using a small piece of logic in the template.
On reflection EJS was the best choice - it's perhaps one we'll revisit in the future.


## Generated Stylesheets


Prior to beginning work on this project, my only experience of generated stylesheets was SASS when combined with the Sencha Touch framework. While it solved a problem, I hated the fact that I had to run a command to re-compile my stylesheets upon making any change.
Express has a few generated stylesheet options - the difference is it uses it's middleware to monitor for changes to the style file, and if it's changed, automatically generates the .CSS output on the next page request.
The main options were Stylus and LESS, both of which have middleware available for Express. Stylus is part of the "who needs curly braces and semicolons" school - not for me. LESS, on the other hand, is still valid CSS - the choice was obvious.
Less, when combined with Express, is incredibly powerful.Things get even more interesting when we add Bootstrap.


## Bootstrap apps all look the same...


Picture an entire web-app, who's look & feel can be modified with [one line of CSS](https://github.com/feedhenry/fh-studio/blob/master/client/css/main.less#L23). That's exactly what we built.
Bootstrap's CSS is based on LESS, meaning it plugged into the existing LESS stylesheet of our application with a simple import.

Combined with the fact that the outer frame of our studio was largely hand-rolled, this negates an issue often present with a project that uses Twitter's amazing Bootstrap framework - things no longer look the same.


## 




## 




## A Single-page App, only not!


One of the biggest challenges of writing a big web application is handling the framework around page navigation. Wouldn't it be great if you could just wire up <a href="/somePage"> tags as normal?

Perfectly achievable! When the app gets initialized, an event handler is attached to all anchor links. This handler decides if, based on the URL, it needs to do a single-page navigation or a full page reload.
We do this by appending a class to all links suitable. We also cancel the event handler if somebody has the CMD, Shift or CTRL keys pressed. This means links still function perfectly opened in a new window, and the current tab doesn't navigate - a huge point of frustration for many single page apps!

Once we've decided we're going to do a single page navigation, we request from our Express route a JSON representation of this page. It returns the data, and what template we need to lookup to render it. This is best illustrated with a code example.



<table cellpadding="0" cellspacing="0" >
<tbody >
<tr >

<td width="100%" >

    
    // attach an event handler to all links with the singlepage class
    // that is all links we want to not do a full page nav
    // where possible



    
    $('a.singlepage').click(function(e){
      e.preventDefault();
      var url = $(this).attr('href'),
    
      // Continue as normal for cmd clicks etc
      if ( event.which == 2 || event.metaKey || url==="#" || !url ) { 
        return true; 
      }



    
      // Now do a call to our JSON API made by Express.
      // Because the resource type is JSON, we get JSON back!
      $.get(self.url + ".json", function(res){
        // Res contains the template we need to look up,
        // and the data to apply to it.
        var tpl = res.tpl, // the template name we need for this view
        data = res.data; // the data to apply to the template
        dust.render(tpl, data, function(err, out){
          // DustJS has rendered our HTML. Use JQuery to append it.
          $('body').html(out);
        });
      }).error(function(){
        alert("Error Changing Page");
      });
    });



</td>
</tr>
</tbody>
</table>





This means every page can be rendered from scratch, or as a single-page app navigation. Every page is addressable, and whatever you see in the address bar is the valid URL for that page.

Death to the Hashbang URL? Sure - however recent trends are moving away from AJAX single-page applications altogether. In retrospect, I think we've over-engineered this one. The only piece of the app which really needs to asynchronously load server responses is the editor itself. Opening files within the editor, switching tabs, saving and deleting should be all async - the rest? Why bother! A responsive web server, with caching, should be well capable of serving up page loads response enough to make the difference near imperceivable.


## WebSockets


Traditionally, building an app in an IDE can be a pretty slow and resource intensive process. The FeedHenry App Build Farm takes this strain away from the user, returning a zipped binary which can be installed straight onto your device. Whilst we could poll for the progress of this task, Socket.IO makes doing so over WebSockets completely painless. If the user's browser doesn't support WebSocket connections, the transport mechanism automatically falls back to polling.



So - building a studio in Node.js, can it be done? Absolutely. It's been a great chance to tinker with emerging technologies, and evaluate a plethora of Node modules. We've learnt a lot along the way - if you've tried to do similar, and have anything to add, we'd love to hear from you! Please feel free to reach out in the comments.

_[@cianclarke](http://www.twitter.com/cianclarke) leads FeedHenry’s development of complex mobile apps build in web technologies. He is author of many Node.js modules and contributes actively to the Node.js community._

  *[CLI]: Command Line Interface
