---
author: cian.clarke
categories:
- Developers
comments: false
date: 2012-09-03T10:56:44Z
link: http://www.feedhenry.com/agile-in-a-services-based-team/
slug: agile-in-a-services-based-team
title: Agile in a Services Based Team
url: /agile-in-a-services-based-team/
wordpress_id: 2820
---

Practicing agile on an engineering product is something which has been well documented and practiced worldwide - but introducing agile approaches to a services based team brings its own set of challenges. Here's some of the challenges we've come across, and how we've dealt with them here at FeedHenry.


## Iteration velocity is not a constant


When building a product, eventually the team will reach a plateau where iteration velocity can be accurately measured. It becomes easy to define just how much can be delivered in N number of weeks, before it's time to sit back, review, and plan the next set of deliverables.

In a services based environment, this isn't always the case. Iteration velocity varies from project to project, and depends on the duration, scale, and level of developer experience. Velocity can vary massively, from wildly productive to utterly blocked by an API that hasn't become available. The team, and the agile process, needs to be able to adapt to this consideration.


## Team size varies


Agile product development usually centers around a core team of developers, who may be working on separate features. In a services team, we're still one team - but project assignment can change from week to week, depending on project workload. Ideally, a developer is assigned to one project throughout its course - but in the event of blockers the team needs to be pretty flexible.


## Scrum


When it comes to scrum, the rules stay pretty much the same. It might seem that when different team members are working on different customer projects, what benefit could a group standup possibly bring - but in reality, we're often solving similar problems in a slightly different shape or form. Scrum is hugely beneficial, just the same as it would be in a services environment.
The logistics don't differ either - updates are sub-30 seconds, scrum master is rotated & is responsible for keeping on top of any blockers.


## The traditional rules still apply




### 1) Unit Tests


Unit tests are still an essential part of the development cycle of a services team. In particular, unit testing the cloud side integrations of a project can be a huge lifesaver. In the event something goes wrong, the unit tests can often help quickly pinpoint the exact source of a problem. You can read more about how we [unit test cloud applications here](http://www.feedhenry.com/2012/08/unit-testing-a-node-js-backend/).

One of the things we've recently worked hard on improving is how we unit test client side JavaScript logic - the part of the app that sits on device. In particular, we've focused on testing any complicated pieces of business logic that exist on the client. The principles are similar to those of cloud unit tests - given a certain input X, we expect output Y succeed and return these properties.





### 2) Code Quality


In a services based team, we occasionally find ourselves working with development partners for certain phases of a project. Developers often have different coding styles, and vary their strategy of indentation type and length - so we need to enforce a uniform code style across the team. In addition to this, we mandate an IDE which is capable of running JSHint code quality tests within the editor.






### 3) Pulling it all together: Continuous Integration


In a services environment, the list of projects to be added to the Continuous Integration box is ever growing. A project is developed for a period of N weeks, then released & the codebase is static until some future phase of the project. This can be quite different to a product based team, and it means the setup cost of adding an app to a Continuous Integration server has to be incredibly low for the developer. To ensure this, our CI setup process simply involves the developer creating a simple shell script which can test various parts of the application. A typical build.sh script might look like this:



<table cellpadding="0" cellspacing="0" >
<tbody >
<tr >

<td width="100%" >





#! /bin/bash







cd cloud




echo 'Installing dependencies'




npm install -d




export NODE_PATH=$PWD




echo 'Running npm test'




npm test




cd ..







</td>
</tr>
</tbody>
</table>



Our continuous integration server ([Jenkins](http://jenkins-ci.org/)) then runs this shell script, which sets the current execution environment of Node to the cloud directory (to ensure relative paths work as expected).
Once it's ready to run the node code, it pulls together 1) and 2) above - running the unit tests, and JSHint.

In the event the tests fail, a lead developer (or the whole team) get emailed a report and the build is marked unstable.


### 


If you're actively practicing agile in a services environment, we'd love to hear how you've managed this in your team! Any comments or suggestions, please reach out in the comments.

_Cian leads FeedHenry’s development of complex mobile apps built in web technologies. He is the author of numerous Node.js modules, and is an active open source contributor. Find him on twitter [@cianclarke](http://www.twitter.com/cianclarke), or [github](http://www.github.com/cianclarke)_
