---
author: cian.clarke
categories:
- Developers
comments: false
date: 2012-02-27T10:51:02Z
link: http://www.feedhenry.com/sencha-touches-the-feedhenry-cloud/
slug: sencha-touches-the-feedhenry-cloud
title: Sencha Touches the FeedHenry Cloud
url: /sencha-touches-the-feedhenry-cloud/
wordpress_id: 2814
---

### By Cian Clarke, Software Engineer, FeedHenry


According to Gartner’s recent review of the top technology trends for businesses in 2012, “The user interface (IU) paradigm in place for more than 20 years is changing. UIs with windows, icons, menus, and pointers will be replaced by mobile-centric interfaces emphasizing touch, gesture, search, voice and video.” The disruption created by smartphones, tablets, mobile apps and the resulting consumerization of the enterprise poses new challenges for enterprises as they develop and deploy mobile apps. Traditionally, enterprise solutions have been more focused on complex backend systems integration and less so on frontend graphical interfaces. Complexity and performance became the overriding goals, often at the expense of simplicity and usability. Cool, simple and user-friendly apps in the consumer space now set the scene for enterprise apps. The challenge for enterprise app developers is to create polished apps, integrate these apps with backend systems and other services and deliver apps that are simple and usable while also secure and effective.

Feedhenry has always placed great importance on achieving a native look and feel to cross-platform applications built on our AppStudio. The overhead of developing application UI from scratch in standard web technologies has always proved time consuming, and a great overhead to the development cycle of a project. We’ve experimented with mobile UI frameworks, with mixed results – until we discovered Sencha Touch.


## Why Sencha?


Immediately, all our requirements for native performance in a UI had been satisfied. The framework provides a huge range of UI components, and the extensible nature of the framework means that any missing or nice-to-have component is either already available in the community, or can be easily developed as a user extension. The API documentation is comprehensive and the source code reads like a book.


## Build-Once, Deploy Anywhere?


The Sencha framework is remarkably well aligned to Feedhenry’s build-once, deploy-anywhere strategy. Combining Sencha code with the Feedhenry AppStudio support for packages, means we can use a clever system of overrides on a per-platform basis. This allows users to develop stylesheets specific to Android or Blackberry, which only get bundled for builds of that particular platform.

Ongoing development & instant testing is also facilitated by AppStudio . You can kick off a build for iOS, Android or Blackberry and deploy to your device in the space of minutes. Users can even build an embed tag for hosting as a mobile website.


## Using Sencha with the Feedhenry Cloud


At Feedhenry, we are focused on enterprise mobility and as such have to deliver a deeper and broader set of services to developers building on out platform. For this reason, we have always employed cloud technology so that apps built for enterprises benefit from caching, access management, scalability and more. We encourage our clients to either build their data in the cloud or broker their data through our cloud, reducing load on the user’s backend service. This enables developers to spend more time writing code, and less time spinning up new infrastructure.

The Feedhenry cloud fits perfectly with the Sencha model of building a user interface. Sencha provides a whole range of useful data proxies for loading data through the web, and we felt that a Feedhenry proxy would be very useful – hence, our FHActProxy.



The Feedhenry action proxy allows users to hit Feedhenry cloud endpoints in their code, and bring the data back into a list. We pass the filters and sorters to the cloud, so heavy data bashing can take place cloud-side. It’s available today to clone in Github.

Another interesting possibility which a cloud-connected Sencha app can bring is the ability to serve the definition of a User Interface from the cloud. This is particularly useful to apps published in an app store or marketplace, where conventionally the user interface is rigid, and any changes require shipping an update to the store.

With the Feedhenry Cloud, you can determine dynamically the user interface required in the cloud, and return it to the client. Simply return an array of objects from a cloud function, and add them to a Sencha panel or container using it’s add() function.

When combined with our cloud connected localisation, this makes a huge portion of the UI of an app highly customizable via the cloud.


## Powerful Enterprise Features


In addition to cloud connectivity and cross-platform build, the Feedhenry platform brings a whole host of other enterprise grade features to the table. Enterprise customers are able to view detailed analytics of their live app, and have access to a powerful suite of Access Rights Management as part of the FeedHenry AppManager.

Customers can also distribute applications in-house on iOS, Android and Blacbkerry. This enables companies to build Sencha apps for an internal sales team unsuited to distribution in the App Store. It’s even possible to build an entire App Installer Catalog of Sencha applications for distribution inside a company.

Feedhenry has made Sencha our UI framework of choice for hybrid apps, and we hope this blog post helps others to realize the power of combining Sencha Touch with the Feedhenry Cloud!



@cianclarke leads FeedHenry’s development of complex mobile frontends in web technologies, connecting these to the cloud. Passionate about developing performant polished apps and an avid Sencha developer, Cian brings the best to mobile and cloud.


