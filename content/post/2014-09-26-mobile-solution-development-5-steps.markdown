---
author: john.frizelle
categories:
- Developers
comments: false
date: 2014-09-26T14:58:22Z
link: http://www.feedhenry.com/mobile-solution-development-5-steps/
slug: mobile-solution-development-5-steps
tags:
- enterprise development
- integration
- micro-services
- mobile app platform
- mobile solution development
title: Mobile Solution Development for the enterprise - the 5 steps to bring order
  to chaos
url: /mobile-solution-development-5-steps/
wordpress_id: 4961
---

# Overview



We're often asked to explain the best approach to take when building complex mobile solutions and how the FeedHenry mobile application platform helps to simplify, manage and streamline the process.<!--more-->

The best way to fully answer this question is with a practical example, so let's walk through a recent use case which perfectly showcases the power of the FeedHenry platform in 5 simple steps.



# What was required



An insurance policy management system with the following components:





  * A Native iOS App


  * A Native Android App


  * A Web Based Admin Portal


  * All backed by a nice, clean, secure Node.js RESTful API Server







# What we had to work with







  * A SAML Identity Provider in the back end network


  * A web server which only returned HTML - i.e no SOAP or RESTful APIs







# How we went about it



By following the basic 5 step plan below, we were able to deliver a working prototype, fully integrated into both the authentication service and the web server within a few days.



### Step 1 - Define the architecture



The first step in any project is to define the architecture of the solution. In this case, we had 3 clients, an API server and some back end systems. For interfacing with the back end systems, we had two obvious options:





  * Connect directly from the Node.js API server


  * Create some micro services for connecting to the back end and interface with these from the Node.js API server



Given the option, it is almost always preferable to go the micro service route



  * It allows for the functionality to be easily integrated and re-used in other projects.


  * It promotes developing smaller, cleaner, more focused modules - following the linux principle of doing one thing well


  * It promotes separation of concerns - allowing more developers to contribute to the effort without stepping on each others toes



So, with an architecture in mind (see below) it was time to start figuring out integration.

![Mobile Solution Architecture](/wp-content/uploads/2014/09/Mobile-Solution-Architecture1.png)



### Step 2 - Create the components



Once the architecture was in place and we had decided which components were going to communicate with each other, it was time to represent this in the platform as projects, apps and services. To do this we created the following components:





  * A Web App for the Web Portal - An Express.js Node app set up for static file serving with the FeedHenry SDK already embedded in the default index.html page


  * A Native iOS App - A standard iOS project with the FeedHenry SDK already embedded


  * A Native Android App - A standard Android project with the FeedHenry SDK already embedded


  * A Cloud App - a Node.js API Server to provide data to the 3 client apps


  * A micro service for the SAML Integration


  * A micro service for scraping the back end web server and returning JSON data



Once all these were created, and the services were bound to our new project, our setup looked like this (notice how closely it maps to our architecture diagram above):

![Platform Component Architecture](/wp-content/uploads/2014/09/Screenshot-2014-09-26-12.22.10.png)



### Step 3 - Define the APIs



With the architecture defined and laid down as projects, apps and services, we had something concrete to work with and had 6 git repos (one for each of the components) that we could start coding against. We had already decided which components were going to communicate with each other so we now needed to define the contracts for those communications - i.e. the APIs. Thanks to the platform support for [API Blueprint](http://apiblueprint.org/) we were very quickly able to define the APIs for the Node.js API server which the mobile and web clients would require. In order to allow client development to kick off as soon as possible, we started out with the APIs returning mock data. This is a great approach for getting "working" APIs up and running in a really short time frame (i.e. within about an hour).

![API Documentation Browser](/wp-content/uploads/2014/09/Screenshot-2014-09-26-10.11.20.png)

Even though the responses from these API's were static mock data (for now), as far as the clients were concerned, they were real, functional, remote APIs. This meant that client development could progress as if the API's were fully functional - no need to start mocking out the APIs for each of the three clients.



### Step 4 - Build out the functionality



At this stage, we were only a few hours in and we already had our architecture nailed down and our touch points (i.e. APIs) defined. This allowed us to get to the fun stuff (i.e. the coding) really quickly, but with everyone already having a clear picture of how the solution was put together and how each piece would communicate. One of the really superb things about this approach is that we could now (within a very short time period) get a full team of developers, with vastly different skill sets (native client developers, web developers, Node.js API developers, Authentication integrators, screen scraping hackers) working together with a minimum of confusion or communication. Each piece of the solution was a stand alone component with it's own git repo so there would be no merge conflicts between the components. Furthermore, the API touch points between the components were already programatically defined, and could be interacted with via the platform using the interactive documentation & discovery functionality.

Over the course of the next several hours, each team member worked on their component with autonomy. At regular intervals, we had each developer push their code back up to the platform's hosted git repositories so we could keep an eye on progress and get working prototypes out into the hands of the customer. Thanks to the platforms hosted build farms, we were even able to co-ordinate the generation and distribution of app binaries.

![Platform Binary Building](/wp-content/uploads/2014/09/Screenshot-2014-09-26-15.01.46.png)



### Step 5 - Integrate the micro services



Once the various micro services were functional, it was time to integrate them into the API server. Again, thanks to API blueprint definitions of the services' API's the API Server developer was able to complete this integration with minimal input from the service developers - the API contracts for the micro services had already been defined and the actual implementation detail of the service was completely transparent to the API Server developer.

Meanwhile, the mobile and web client development was continuing to progress independently against the mock data being returned by the API Server. Once the micro services were integrated, it was as simple as toggling an environment variable (fully managed via the platform) to switch from mock data to live data. There were no changes required on the client - their API contract remained exactly the same, but with the flick of a "switch" the API server was now returning live data.





# Conclusion



Mobile solutions - especially ones for Enterprise - are typically more complex than just an app on a device; even if this is the most visable bit, and the part of the solution which most people tend to focus on. However, it is vitally important to understand and define up front the full solution. By taking this approach, the various components and integration points can be defined early - giving a solid skeleton upon which to build your solution.

Additionally, the use of micro services to isolate and encapsulate functionality - particularly enterprise integration logic - provides an excellent return on investment, both for the initial solution and for all the ones which come after it and leverage those micro services.

When done right, and with the support of a strong mobile app platform, even the most complex mobile solutions can be a pleasure, rather than a challenge, to implement.
