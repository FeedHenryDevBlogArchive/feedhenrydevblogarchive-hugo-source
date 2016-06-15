---
author: cian.clarke
categories:
- Blog
- Developers
comments: false
date: 2015-04-08T13:54:34Z
link: http://www.feedhenry.com/microservices-for-mobile/
slug: microservices-for-mobile
tags:
- microservices
- mobile
title: 'Evolving a Mobile-centric Architecture: The Microservices Way'
url: /microservices-for-mobile/
wordpress_id: 5401
---

There seems to have been an explosion in the use of the term “microservices” recently. I’ve been peripherally aware of the concept for some time now, but it seems it first came to light with a fantastic collection of [thoughts by Martin Fowler](http://martinfowler.com/articles/microservices.html)[1] - some great reading on the topic.<!--more-->

This three-part post will not help you make a business case for rewriting your existing monolith as a series of microservices - for that, I’d recommend [Rich Sharples’ writings on the topic](http://blog.softwhere.org/2015/02/21/microservices-the-new-architecture-for-digital-engagement/) [2] - but I am going to show how microservices make sense for mobile in a series of practical examples.
  





    
  1. A Microservices Primer: Introducing the concepts, along with mobile-specific microservice considerations

    
  2. Hands-on examples how to build some simple microservices

    
  3. A benchmarking exercise revealing the impact microservices-based architectures can have on mobile.



The end result of this exercise can be performance improvements between ~**2.5x **and** ~250x**, depending on network conditions[3]. To see a 10 minute video presentation [click here](http://www2.feedhenry.com/mobile-tech-talk-microservices)



# 1: A Microservices Primer





## Microservices - The Term



I’ve been somewhat amused by the term microservices, since we've been composing small, loosely coupled applications which combine to do work since I first started using Node.js with FeedHenry. I'd love to claim some visionary stroke of trend-predicting genius, but really it’s the path the Node.js community led us down. Of course, it's widely accepted the concept has been around since Unix days - it's just a shiny new name.

In the Node community, what started as cries of “Make everything you possibly can as small re-usable modules” (micromodules anybody?) quickly became "Make everything small re-usable applications,” and now we're calling them microservices.



## Microservices - Considerations for Mobile



Introducing a microservices-based architecture has some specific considerations when it comes to delivering content to mobile applications. I'm going to deal with two main concerns - **coupling and performance**.



### Loose-Coupled, Tight-Coupled, Practically Welded Shut



For web applications, if we change the API we know that once we deploy an update to the web application, all our connected clients are using the new API. It's easy to swap out URLs, and even expected payloads in the code of the web application, because we can deploy in tandem. Then we can deprecate the old API. This makes for a relatively loose coupling between client and server.

Mobile applications are different, however. A mobile app is released into an app store. In an enterprise environment, this app store is often private. We can usually force an update out to our app and watch it propagate to users within a matter of days. This makes the relationship between client and API more tightly coupled than in the case of the web application.

Releasing to the public app store, there may be a review period, but once released, users download this update over the course of weeks, months, maybe never. The previous API still needs to be maintained, and this makes for an integration which is so tightly coupled, it's practically welded shut. (See, these days everybody is coining new terms!)

This makes for some very special considerations when architecting for mobile. The lifespan of any APIs the mobile application touches need to be much longer-lived, and APIs should be built with this in mind. Consider versioning, and some way of forcing users to update.

[![Microservices_diag1](/wp-content/uploads/2015/04/Microservices_diag1.png)](/wp-content/uploads/2015/04/Microservices_diag1.png)

_Mobile release cycles are much longer, leading to tighter coupling between client and API_



### Performance Considerations



The other major consideration specific to mobile is performance. Web applications typically run on devices connected over WiFi or Ethernet, with low latency. Mobile devices often connect over lossy connections - 3G, Edge, or even GPRS.

Returning the minimum payload required to render the screen is more important now than ever. Intelligent pagination on lists can drastically reduce payload size, especially when users are only operating on the most recent items. This also means reducing the number of calls made across the network. As the number of calls grow, every HTTP transaction can contribute to an exponential growth in overall response time. Creating a microservice to act as a unified mobile API, which returns data from many sources in one single call is often a good option.

This can introduce some interesting trade-offs that need to be balanced:




    
  1. At what point does a unified API returning multiple types of data compromise the RESTful nature of an API?

    
  2. At what point does the response body size of a combined payload negate any potential performance gains?





# 2. A Mobile Example of a Microservice



In part 1, we introduced the term, and presented some unique considerations for mobile. Now, let's take a more practical look by creating some microservices.



### The Tale of the Traveling Umbrella Salesperson



To illustrate the use of microservices, let's take a mock use case, that of a traveling umbrella salesperson (of no relation to the Traveling Salesperson of Distributed Computing fame). This salesperson needs to be able to:




    
  1. Create orders in a back-end database

    
  2. Automatically scale their order quantities according to the weather (I know - work with me here, people…)

    
  3. Notify the account manager of the new order on their account





### Our first Microservice - Orders



First, we're going to create a microservice for orders for our traveling umbrella sales team. We're going to write our microservices in Node.js, and they're going to communicate JSON payloads over HTTP, but these are by no means prerequisites. Microservices can of course be implemented using any programming language, over any communication protocol.

Here's a service which can both create and list umbrella orders. It creates a REST API, `/orders`.


    
    var app = require('express')().use(require('body-parser')());
    var orders = [];
    // Create a new order
    app.post('/orders', function(req, res){
      orders.push(req.body);
      return res.json(req.body);
    });
    // list orders
    app.get('/orders', function(req, res){
      return res.json(orders);
    });
    
    var server = app.listen(3000);
    



Ten lines - pretty micro, huh? Ok, so I cheated a bit - that first line is concise to the point of unreadable, and we're just putting orders in memory - but we now have a really micro service.

_For future code snippets, I'm going to drop some of the boilerplate setup code on the first and last lines_



## Adding more Microservices - Weather and SMS



Our order service is working just fine - our folk out in the field can create orders, and list the orders they've previously created. But before the team creates an order, they want the system to scale their order size based on the upcoming weather forecast at that location. Let's call this the rain service. Here, we're going to reach out to a third party API, sum the rainfall totals for the upcoming forecast, and append it to the original weather information.




    
    var weatherUrl = 'http://api.openweathermap.org/data/2.5/forecast';
    
    app.get('/rain', function(req, res){
      var city = req.query.city,
      country = req.query.country;
    
      request.get({url : weatherUrl + '?q=' + city + ',' + country, json : true}, function(err, response, weatherbody){
        // sum all the inches rainfall in the forecast
        weatherbody.rainfall = _.reduce(weatherbody.list, function(a, b){ return a + b.rain['3h'] }, 0);
        return res.json(weatherbody);
      });
    });
    



Lastly, we're also going to add a service to allow us to push an SMS alert to the account manager when a new order is created.


    
    app.post('/sms', function(req, res){
      var to = req.body.to,
      message = req.body.message;
      client.sms.messages.create({ to: to, from : process.env.TWILIO_NUM, body : message}, function(error, message){
        return res.json(message);
      });
    });
    app.listen(3002);
    



We've now created our series of three microservices. To get started with the examples provided, follow these steps in a terminal:


    
    # clone the repository
    git clone https://github.com/cianclarke/microservices-primer.gitcd microservices-primer
    # Install dependencies
    npm install -d
    # Set Twilio environment variables
    export TWILIO_AUTH=foo; export TWILIO_SID=bar; export TWILIO_NUM="+1234567";
    # start the 4 microservices & the test runner
    npm start
    # To view the test runner, visit http://localhost:3004/ in a browser. 
    



We're now running our series of microservices, and can interact with them using CURL


    
    # Service 1: Create a new order in the database 
    curl 'http://127.0.0.1:3000/orders/umbrellas' -H 'Content-Type: application/json' --data-binary '{"city":"Dublin","country":"Ireland","quantity":984.4999999999999,"accountManager":"+1 123 456 789"}' 
    # Service 1: List orders in the database 
    curl 'http://127.0.0.1:3000/orders/umbrellas'
    # Service 2: GET request to rain svc to retrieve info for Dublin 
    curl 'http://127.0.0.1:3001/rain?city=Dublin&country=Ireland' 
    # Service 3: POST to the SMS service 
    curl 'http://127.0.0.1:3002/sms' -H 'Content-Type: application/json' --data-binary '{"to":"+1 123 456 789","message":"My SMS Message!"}' 
    



We've now built a series of microservices, and verified we can interact with them.3: Taking our Microservices MobileNow, let's look at two different approaches to consuming these. The first will show a more traditional approach to building out the application. The second will introduce another microservice specifically for mobile, taking into account the mobile-specific concerns raised in part 1. Then, we'll look at some results of our benchmark.



## Take 1: Client-Side Business Logic



First, we'll build this application the way many existing mobile apps are built - we'll implement a lot of business logic on the client (scaling the order according to the rainfall microservice, submitting to the orders microservice, then notifying the account manager via the SMS microservice), and make three separate REST calls from the mobile device.Sure, we've still got microservices on the server-side - but we could equally picture this as a monolith, for what little use we're making of the microservices philosophy.[![microservices_2](/wp-content/uploads/2015/04/microservices_2.png)](/wp-content/uploads/2015/04/microservices_2.png)

This illustrates the "wrong way."




    
  * We’re making several calls from the app, so we’re not minimizing our number of requests.

    
  * None of the calls have been optimized for mobile. We’re sending back unnecessary data, so we’re not trimming the payload for mobile.





## Take 2: Microservices for Mobile



As before, we're going to achieve this integration using a series of microservices - but we're going to meld the data together in a fourth and final mobile-specific microservice.[![microservices-3](/wp-content/uploads/2015/04/microservices-3.png)](/wp-content/uploads/2015/04/microservices-3.png)

Now, we send our order information to a new mobile specific microservice which wraps up the three steps we took in “take 1.” We’re now making one call from the mobile device. We've got one simple API to maintain, a perfectly reasonable POST of a JSON payload, and this integration becomes a much looser coupling. We're not sending unnecessary data back to the mobile app.



## Benchmarking – Client-side Logic vs. Microservices approach across mobile networks



Now, let's see how these two approaches compare across a range of mobile network types. If interested, some notes on the benchmarks are available in the footnotes[3].

A simple mobile client implementing the two approaches to integrating discussed above was created, which also measures request time across each approach.



### Payload Size



First, a brief look at the total payload size over 100 requests. Using the mobile microservice, **68kb** of data was exchanged. Calling each microservice individually resulted in **1.5mb** of data, a substantial increase likely due to returning weather information in it's entirety on every request.



### Response Times



[![Response Times](/wp-content/uploads/2015/04/responseTimes.png)](/wp-content/uploads/2015/04/responseTimes.png)

Examining the results, we see nominal difference between approaches using a 4G network. When we move to a 3G network, the gap widens - there's eight** seconds** in the difference across requests. Edge networks show a wider gap still, with an increase in average request time of almost **27 seconds**. At this point, the end user would really feel the pain of an inferior architecture. Lastly, over GPRS the impact of implementing multiple calls on the client is eye-opening. With average request times of **more than 2**** minutes**, the application would be effectively unusable. Compare this to a microservice based architecture, with response times averaging 12 seconds, the application is slow but still usable. The key take away from this benchmark is that as the network slows, **response times grow exponentially** when not considering mobile in your microservice architecture.



## Conclusion



Having seen the results, what can we conclude from these benchmarks? I'm not going to try make such bold, linkbait-esque claims as "Microservices make mobile 10x faster" - that's simply not true. What is true, however, is that the roll-out of a Microservices based architecture needs to consider mobile as a first class citizen. Not doing so can ruin the user experience for end users, and render applications virtually useless on slower networks. If the above considerations are taken into approach, the rollout of this new breed of architecture should prove a much smoother transition.



# Further Reading



[1] Martin Fowler - Microservices. The post which for many started the momentum behind the term.
[http://martinfowler.com/articles/microservices.html](http://martinfowler.com/articles/microservices.html)

[2] Rich Sharples - Micro services – the new architecture for digital engagement? Some observations from a higher level on the trends and applications of microservices.
[http://blog.softwhere.org/2015/02/21/microservices-the-new-architecture-for-digital-engagement/](http://blog.softwhere.org/2015/02/21/microservices-the-new-architecture-for-digital-engagement/)

[3] 100 requests in each run. SMS API calls to Twilio simulated with 250ms timeout to avoid excessive calls to their API. Chrome Developer Tools network throttling used to simulate network speeds. Node.js processes restarted after every batch of 100 requests. Requests had timestamp appended to prevent browser caching.



Watch a powerful [10-minute video](http://www2.feedhenry.com/mobile-tech-talk-microservices) on this content that includes a live demo of the benchmarking exercise.







_[@cianclarke](http://www.twitter.com/cianclarke)[ ](/wp-content/uploads/2014/06/Screen-Shot-2014-06-11-at-15.56.07.png)is a Software Engineer with FeedHenry. Primarily focused on the mobile-backend-as-a-service space, Cian is responsible for many of FeedHenry’s mBaaS developer features._






  *[mBaaS]: Mobile Backend As A Service
