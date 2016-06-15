---
author: david.martin
categories:
- Developers
comments: false
date: 2012-08-24T10:58:09Z
link: http://www.feedhenry.com/so-long-jquery-ui-hello-bootstrap/
slug: so-long-jquery-ui-hello-bootstrap
title: So long jQuery-UI, hello Bootstrap
url: /so-long-jquery-ui-hello-bootstrap/
wordpress_id: 2822
---

## The way things were...


The FeedHenry App Studio in its old form was built primarily on jQuery, jQuery-UI and numerous jQuery plugins. The main plugins used were Accordion, Tabs, Button, Dialog, jWizard, jqGrid and UI-Layout. On top of this, there was a tonne of custom CSS classes, id's and styles. It was (and still is) a big project with a lot of files. Despite this, it did what it was required to do i.e. Provide a frontend to the FeedHenry Cloud platform.


## Why change? It works!


Yes, it did work, but had some problems. It had problems from both the User's (your) and Developer's (our) point of view. The UI was dated looking and needed some love. We made a choice a few years back to create the studio as a single page application (jQuery UI-Layout). As it evolved, this choice proved troublesome as we added new elements like metrics and reporting. In fact, any new features that were added had to fit strict size constraints. It was a pain, but we did it nonetheless.

jQuery UI was great at the time, but didn't do enough for the general look and feel of the Studio. We still ended up adding custom classes and styles for a lot of things. This led to confusion between class conventions and patterns, and ultimately led to multiple classes all doing similar things. The level of selector reuse was bad.

The javascript was a similar story. There was no strict pattern followed throughout the Studio code. Instead, there were small pockets of patterns as various features were added over the years. Some patterns matched, but in general the code was badly structured. We used John Resig's javascript inheritance 'Class.extend' [http://ejohn.org/blog/simple-javascript-inheritance](http://ejohn.org/blog/simple-javascript-inheritance/) in some parts, and functions with public & private members in other parts (similar to how Douglas Crockford describes private members [http://javascript.crockford.com/private.html](http://javascript.crockford.com/private.html)). There were some nice sub-patterns in the code, particularly around the auto creation & binding of the tabs ('north' layout), each with an accordion (in the 'west' layout) that controlled the main content shown in the 'center' layout. As it was a single page application, the footer sat in the 'south' layout.

From a User's point of view, the UI needed to be overhauled to give a nicer experience. From our point of view, the code needed restructuring and simplifying to make maintenance easier and more fun. A good knock-on-effect of this, as we discovered, would be speed improvements, particularly the initial load time of the Studio and the time it takes to switch tabs.


## Out with the old...


We used Bootstrap [http://twitter.github.com/bootstrap/](http://twitter.github.com/bootstrap/) before for the Docs site [http://docs.feedhenry.com](http://docs.feedhenry.com/) and landing pages on [https://hpcs.feedhenry.com](https://hpcs.feedhenry.com/) and [https://mobilecf.feedhenry.com](https://mobilecf.feedhenry.com/). It worked well and provided a nice UI layout out of the box. It was a good fit for what we wanted to do with the Studio UI. So while we replaced the old jQuery UI components with the shiny new Bootstrap equivalent, we also untangled and rewired as much of the coding mess as possible. We made a big leap towards an MVC pattern everywhere.

For those who are interested in what we replaced the jQuery UI/plugin components with, here's a table:
<table cellpadding="0" cellspacing="0" style="width: 75%;" border="0" >

<tr >
jQuery
Bootstrap
</tr>

<tbody >
<tr >

<td >UI-Layout
</td>

<td >Fluid Grid System
</td>
</tr>
<tr >

<td >UI-Tabs
</td>

<td >Basic Tabs
</td>
</tr>
<tr >

<td >UI-Accordion
</td>

<td >Nav list
</td>
</tr>
<tr >

<td >UI-Button
</td>

<td >Buttons/Button Groups
</td>
</tr>
<tr >

<td >qTip
</td>

<td >Tooltip/Popover, Labels
</td>
</tr>
<tr >

<td >jqGrid
</td>

<td >dataTables
</td>
</tr>
<tr >

<td >Dialog
</td>

<td >Modals
</td>
</tr>
</tbody>
</table>


Also, some other Bootstrap components we used in place of our old custom code/styles:



	
  * Breadcrumbs (custom styles before)

	
  * Forms (custom form & input styles before)

	
  * Font-Awesome icons in nav list (no icons before in accordions)

	
  * Transitions (no transition before, anywhere)





## Where did the magic go?


Wizards haven't changed at this time. There's still one old plugin in use because the migration for it is a big task on its own. The jQuery jWizard plugin is heavily integrated into the UI. The plugin was also modified to allow showing and hiding buttons at different steps in the wizard. Before we make a decision on this, we need to ask ourselves if wizards are the most appropriate solution. Should we even use a modal dialog for them (we don't in some places already)? Should all the wizard steps be presented on a single screen (currently only do this for 'Generate App')? Watch this space...
