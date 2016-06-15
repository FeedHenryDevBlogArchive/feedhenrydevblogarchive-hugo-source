---
author: cian.clarke
categories:
- Developers
comments: false
date: 2012-08-16T10:59:04Z
link: http://www.feedhenry.com/unit-testing-a-node-js-backend/
slug: unit-testing-a-node-js-backend
title: Unit Testing a Node.js Backend
url: /unit-testing-a-node-js-backend/
wordpress_id: 2824
---

Unit tests are an essential part of the lifecycle of any app development project, and today we're going to discuss the use of cloud based unit tests. Using the FeedHenry platform as a mobile Backend as a Service (mBaaS), and writing integrations with complex systems as a series of cloud based services, is an excellent strategy for developing a series of mobile applications with modular, re-usable integrations.

However, these integration points become a vital interface between your backend systems and the rest of the world. Every line of cloud logic could potentially affect hundreds of thousands of connecting mobile clients, so this code is of vital importance. For this reason, having a substantial test coverage rate is critical.


## Writing tests


A myriad of options exist when choosing a testing framework. The frameworks vary in their delceration style ( BDD, should(), expect() and plain assert() ), and also the reporters used to return the results (Spec, Plaintext, JSON, ...). The most popular unit testing frameworks are [Expresso](http://visionmedia.github.com/expresso/) (no longer actively maintained, but in widespread use), [Mocha](http://visionmedia.github.com/mocha/) (superceeds Expresso), and [VowsJS](http://vowsjs.org/). Choice of framework depends largely on personal preference, and a train of thought exists that would suggest framework free, plain JS tests are the best approach. Whatever the choice of method, the core concept remains the same - verify that with some input, we receive an expected set of output.

For the purpose of this article, we're not going to use a framework. Instead, we're going to use the assert module, which comes with NodeJS by default, and we're going to write a simple set of assertions that, combined with some console logging, verify our cloud code does as we expect.


## Coverage


In a FeedHenry application, all functions that exists in the cloud/main.js file that are exported are public. For example

    
    exports.a = function(params, callback){
        return callback(null, {a: 1});
    }
    function b(params, callback){
        return callback(null, {b: 2});
    }
    exports.b = b;
    function _c(params, callback){
        return callback(null, {c: 3});
    }




In the above example, function a and function b are exported - so these are public functions, accessible from mobile devices.
As a bare minimum, these publicly exported functions should both be tested. Function _c is not exported, so doesn't have to be tested - in fact, since it's not available outside this file, we can't test it.




In addition to testing all the exports that exist in main.js (i.e. all those available to out mobile devices), often we will have some utility modules that we might write to assist with data massaging, parsing, or integrating. Testcases should also exist for any exported functions that are a part of these modules.










## The Tests


We create our tests in a file within cloud/test/tests.js:

    
    var main = require('../main'),
    assert = require('assert');



    
    main.findStores({ query: '' }, function(err, res){
       console.log("findStores: Running tests");
       assert.ok(!err);
       assert.ok(res);
       assert.ok(res.data);
       assert.ok(res.data.length > 0);
       var data = res.data,
       aRecord = data[0];
       assert.ok(aRecord);
       assert.ok(aRecord.county);
       assert.ok(aRecord.address && typeof aRecord.address === "object");
       assert.ok(aRecord.distance && typeof aRecord.distance === "number");
       console.log("findStores: Tests passed OK.");
    });


This is a simple example which invokes the findStores function of main.js with one paramater - a query string of '', and asserts the existence of & type of a number of properties in the response data.


## When to run?


Here's some scenarios where running a unit test suite can 'save your bacon':



	
  * Before you check in a change that modifies a file in /cloud
_You'll catch an error before it's even made it into the repository, never mind into production. The code is fresh in your head - the error is easy to fix. _

	
  * You receive an alert from your monitoring system saying "stuff is down"
_Your unit test suite will serve as a quick sanity check, pinpointing the line of code where 'stuff went wrong', and should tell you if the issue is with your code, the endpoint you're integrating with, or otherwise.._

	
  * Running on a Continuos Integration box
_A change was checked in without running the test cases, or the data returned by the endpoint we integrate with has changed & we haven't managed to handle it. Bad - but at least we've caught the issue before we push to production!_




## Screencast


To illustrate the points made, we're now going to write a small unit test for one of our example apps, [Store Finder](http://github.com/feedhenry/Store-Finder).

[You can view the screencast on vimeo](https://vimeo.com/47468233/)
