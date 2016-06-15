---
author: conor.oneill
categories:
- Developers
comments: false
date: 2014-07-10T11:06:49Z
link: http://www.feedhenry.com/creating-re-usable-mbaas-services-feedhenry-3/
slug: creating-re-usable-mbaas-services-feedhenry-3
tags:
- api blueprint
- MBaaS
- microservices
- services
title: Creating re-usable mBaaS Services in FeedHenry 3
url: /creating-re-usable-mbaas-services-feedhenry-3/
wordpress_id: 4461
---

One of the most powerful features in FeedHenry 3 is the ability to deploy mBaaS Services that can be accessed by multiple projects. This means you can, for example, encapsulate all of the functionality to access one of your back-end systems and then provide a consistent interface to it for all of your projects. This also facilitates a separation of concerns between those building mobile apps or Node.js business logic and those creating the services for them. The consumers of the services never need know about the nitty gritty of how back-end systems are accessed, they just develop against the provided APIs.

In this video, Jason walks you through the mBaaS Service functionality and you will see the types of efficiencies possible by taking a Microservices approach.



In addition to using one of our off-the-shelf services, you can easily generate your own custom mBaaS Services by creating an [Express 4](http://expressjs.com/) Node.js application and documenting the RESTful API end-points you wish to expose with [API Blueprint](http://apiblueprint.org/). Doing this gives your Service consumers a dashboard in FeedHenry 3 which provides both the information needed to use the service and an interactive tool to exercise the service directly.

![oracle_service](/wp-content/uploads/2014/07/oracle_service-1024x504.jpg)

Making developers more productive was one of FeedHenry 3's biggest drivers. With mBaaS Services, they can mobilise their back-end systems once, rather than re-inventing the wheel on each new project.
