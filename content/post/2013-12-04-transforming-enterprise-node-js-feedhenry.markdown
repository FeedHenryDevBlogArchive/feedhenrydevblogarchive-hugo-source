---
author: conor.oneill
categories:
- Blog
- Developers
comments: false
date: 2013-12-04T18:11:23Z
link: http://www.feedhenry.com/transforming-enterprise-node-js-feedhenry/
slug: transforming-enterprise-node-js-feedhenry
tags:
- business
- node
- node.js
- nodeconf
- transformation
title: 'Transforming the Enterprise with Node.js '
url: /transforming-enterprise-node-js-feedhenry/
wordpress_id: 2944
---

**_By Damian Beresford - Engineering. Conor O'Neill - Product Management. Joe O'Reilly - Solutions Architecture._**

Node.js has gone in a few short years from an interesting twist on server-side JavaScript to the engine of change in Enterprise Architecture. This blog post explores the market trends in using Node.js as a core tool for improving, mobilising and potentially migrating Enterprise legacy systems. It amalgamates our own experiences in integrating with legacy applications  and how we've helped our Enterprise customers begin the mobilisation of legacy systems and evaluate candidates for moving to a next generation platform. It also draws upon recent case studies and conference talks from companies such as PayPal, Walmart, Ebay, Groupon, etc who are using Node.js in production to run their businesses.


## Mobile


Mobile is the chief driving force behind new requirements for these Enterprise Systems. The internal and external demands for attractive, high-performance apps increases daily. The work force also expects all internal systems (web applications in particular) to work well on mobile phones and tablets. Current systems are struggling to keep up with this flood of mobile requirements and the cutting edge technology demanded of them.

[![Traditional_Mobilisation](/wp-content/uploads/2013/12/Traditional_Mobilisation.png)](/wp-content/uploads/2013/12/Traditional_Mobilisation.png)


Fig 1: Traditional mobilisation of an existing Enterprise System





## Node.js as an emerging best-of-breed Enterprise solution


FeedHenry provides a Mobile Application Platform which provides all of the end-to-end tools and infrastructure you need to build Cloud-powered Mobile Apps. Node is a critical part of our platform and enables our customers to connect mobile apps to complex Enterprise systems without drama.

We fundamentally believe that Node.js is the best tool for enabling organizations to meet the requirements of cutting edge Enterprise Mobile development. We, among many others, find Node.js to be an order of magnitude faster than other tools for developing services.

We highly recommend you watch this "[Releasing the Kraken](http://www.youtube.com/watch?v=V5yk5SZxWX4)" video by PayPal's Bill Scott at the recent Nodeconf EU which FeedHenry sponsored. Some of his metrics around efficiencies gained are genuinely shocking. Also check out the great post about it on the [nearForm blog](http://www.nearform.com/nodecrunch/?p=109).

Jeff Harrell of PayPal has followed it up [with another post](https://www.paypal-engineering.com/2013/11/22/node-js-at-paypal/) and Eran Hammer of Walmart [live-tweeted](https://twitter.com/search?q=%23nodebf%20%40eranhammer&src=typd) the zero-drama nature of running all of their mobile traffic through Node on Black Friday. 


## Legacy Systems - Node Proxy as a first step


Node can initially be used to implement the '[Smart Proxy](http://www.eaipatterns.com/SmartProxy.html)' pattern which can be applied to migrating requests from an old System to new System services. Node.js makes for a highly efficient request proxy. The idea here is that Node sits in front of your existing Enterprise Systems (usually a portal server, e.g. Apache/Tomcat) and simply routes http traffic to its correct destination.This pattern also helps with API backward compatibility, e.g. for a new service that replaces a part of the old System.

From a development perspective, the first version of Proxy can be a simple pass-through, i.e. just forward each request to the back end without any routing/modification. Not only will this give you a foothold in production, but also confidence that there is only a minimal performance overhead with the proxy (~10 milliseconds).

As a bonus here, the Proxy can log all API requests to the backend allowing you to effectively see what parts of the backend API are being used most frequently. Once the initial version is embedded in Production, it’s then time to start routing requests to new services and start consolidating API’s from various different sources.


## Node and development of micro business services


Node is purpose-built for developing small web services. In fact, even the "hello world" example on nodejs.org is a simple web server. And thanks to the Node community, a huge number of NPM modules exist for all sorts of integrations. There are also various Node.js based frameworks for assisting with micro service development, e.g. hapijs.com.

In Node, these micro services should consist of several smaller component modules which are versioned and used in node via NPM. These modules can be shared with other services, also via NPM, allowing you to implement a truly _component-based architecture_.

Architecturally, depending on your system boundaries, these business services may be new business requirements, or replacing a part of the old System that had heavy technical debt. Also bear in mind that you may never get to replace all of an old System, due to legal or other complications.


[![Proxy_Micro_services_03](/wp-content/uploads/2013/12/Proxy_Micro_services_03.png)](/wp-content/uploads/2013/12/Proxy_Micro_services_03.png)




Fig 2: Moving to proxying and micro-services with Node





## Enterprise System Migration


CIOs are under constant pressure to deliver new mobility-focused services whilst preserving the integrity of existing mission-critical systems.  Many Enterprise systems are well architected, resilient and scalable under load but many others can be monolithic and brittle in nature, fuelled by years of entropy.

Organizations often mitigate risk by having long and complicated change approval processes and test-release cycles. The systems become ever-more-difficult to change and extend, particularly for core system modules which are essential to overall system stability. It is a serious challenge facing Enterprises to be able to meet high velocity requirements and drive the business forward.

However, these existing systems are the foundation of the Enterprise’s business, and are typically what generates revenue. Migrating large Enterprise Systems in one large rewrite rarely works out well. Typically this is done by forming a new team, who plan to deliver ‘like for like’ functionality in a ‘greenfield’ environment. Often new teams under-estimate the size/complexity of the old system and fail to deliver. Meanwhile, the old system has been held together by a skeletal team, and overall the migration effort has been an expensive set-back to the business.

Instead of the big rewrite, we have found that working with the old system from the start, and then creating new micro-services built around it, is a more successful approach. This is sometimes referred to as a ‘brownfield’ development effort. These new services are then either replacement parts of the old System, or services purpose-built for new business requirements.

Eventually over time, the old system can get smaller and less core to the overall architecture, whilst these new micro-services grow more numerous in number and also in depth of functionality.

Having smaller decoupled, autonomous micro-services is a more desirable modern architecture. It allows for quicker development and testing, less risk on production stability, and faster/better turnaround of requirements, allowing the development teams to focus on adding business value.


## Flexible Cloud-hosted Node.js backend


FeedHenry made the move to Node.js a long time ago (in Node years) at the beginning of 2011 when it was at v0.4. Our flexible Node-powered backend enables you to quickly and easily develop Node.js services that can interact with your existing business systems on one side and your mobile apps on the other. Using the FeedHenry cloud, you don’t need to expose your existing systems directly on the internet in order to interact with mobile applications. Instead, your mobile applications talk to the proven robust, scalable infrastructure of the FeedHenry Cloud which then talks securely to your backend systems.

This pattern is in active use today by all of our customers including a major UK rail infrastructure company, a leading airline, large UK utility companies and Government agencies. In every case, existing systems provide both the data sources and sinks for totally new mobile applications with all of the necessary management and manipulation happening securely in the Node.js business logic.


## Professional Services/Consultancy


Our long history with Node is critically important to your business success. Rather than being just another hot technology that has attracted bandwagon-jumpers, we made the strategic decision several years ago that Node should be at the heart of everything we do. In addition to a battle-hardened Node infrastructure, we also have a battle-hardened Professional Services team who have been delivering high-performance robust resilient Node-powered mobile solutions for many years.

But they provide far more than point-solutions and can take you on the journey we have described above from initial micro-services and mobile applications to our full Mobile Application Platform. Our PS team understands that mobility is not just about Apps. By taking a strategic platform approach, they can guide you through the architectural, process and implementation decisions you need to make, to put mobility and agility at the heart of your enterprise and transform your business.


## Large Enterprises making the transition to Node.js


The velocity of Node.js through the software development community has frankly been jaw-dropping. We have never before seen any tech become so important so quickly to so many businesses. What is even more shocking is how large corporations have embraced it and have begun evangelising it both internally and externally. They are doing so because of one reason - Node is transforming the Enterprise.



	
  1. [Groupon](https://engineering.groupon.com/2013/node-js/geekon-i-tier/)

	
  2. [eBay](http://www.ebaytechblog.com/2013/05/17/how-we-built-ebays-first-node-js-application/)

	
  3. [Walmart](http://venturebeat.com/2012/01/24/why-walmart-is-using-node-js/)

	
  4. [The Daily Mail](http://www.nearform.com/nodecrunch/how-node-js-has-revolutionized-the-mailonline#.UpS7eMRdV8E)

	
  5. [IBM](http://nodered.org/)

	
  6. [LinkedIn](http://venturebeat.com/2011/08/16/linkedin-node/)

	
  7. [Microsoft](http://www.slideshare.net/gblock/node-js-enterprise-class)


And [many many more](http://nodejs.org/industry/).


## Summary


By using Node.js in a step-wise fashion, large Enterprises can move at a pace that works for their organisation. A mobility strategy using Node in the modes we have described above can eventually lead to fundamental changes in how you deliver services to your employees and customers.


