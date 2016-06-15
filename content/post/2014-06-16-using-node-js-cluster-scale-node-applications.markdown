---
author: cian.clarke
categories:
- Developers
comments: false
date: 2014-06-16T12:00:29Z
link: http://www.feedhenry.com/using-node-js-cluster-scale-node-applications/
slug: using-node-js-cluster-scale-node-applications
title: Using Cluster to scale your Node Applications
url: /using-node-js-cluster-scale-node-applications/
wordpress_id: 4229
---

Cluster ([API Docs »](http://nodejs.org/api/cluster.html#cluster_cluster)) landed in Node 0.8, but because of it's label as still an Experimental API, it probably hasn't yet received the recognition it deserves. There are downsides to using such experimental APIs - the API footprint of cluster within your application is **very likely to change** in future releases. But, the upsides are massive - multi-process scaling of your application with a few lines of code.

Traditionally, Node.js applications are single threaded, and will operate on one CPU.  Cluster works by scaling your Node application across multiple CPUs. Cluster applications have a master, which controls N worker processes - where N is typically the number of CPUs on your system.
In current versions of Node, the master presently assigns an incoming connection to these works by waking up one of the N workers - but this changes in 0.12. Soon, the master will accept connections, and pass them off to workers using a Round Robin scheduler. Waking up processes can be an expensive operation, so this shift should see even better efficiencies from using Cluster.

I first came across Cluster when trying to scale out large exports and imports of binary data through our data storage component. Thankfully, the component was already cluster enabled - and I'd spent some time figuring why things weren't scaling to multiple workers. Turns out, the VM I was running in had one CPU. Bump the count, and viola - instant scaling!

Cluster does much more than just help scale your code, however - it also adds a degree of high availability.
The key to adding resilience using workers is to keep the amount of work the master does to a minimum, reducing the risk of uncaught exceptions. If your master has an uncaught exception, the entire process is in trouble. If a worker has an uncaught exception, however, we're able to restart the worker.
Along with uncaught exceptions, we also use Cluster to perform configuration changes, upgrades, etc on our Node components without taking the service off the air. By sending our process a `SIG` [signal event](http://nodejs.org/api/process.html#process_signal_events), we can listen for this event and when received restart each of our worker nodes individually.



![AutoScale](/wp-content/uploads/2014/06/Screen-Shot-2014-06-11-at-16.26.54-300x255.png)


## Is it really that simple?


Sort of! Scaling your application using Cluster is pretty straightforward. Our [MBaaS Instance template](https://github.com/feedhenry-templates/helloworld-cloud-cluster), an Express app, is offered with cluster scaling built in. It's the best place to start learning how Cluster works, and we're going to take code snippets from this throughout. Links to highlighted lines from the source code are supplied if more context for the snippets is needed.

Let's start with our existing express app - we've already [included the cluster module](https://github.com/feedhenry-templates/helloworld-cloud-cluster/blob/master/application.js#L5), and we've [setup an express application called 'app'](https://github.com/feedhenry-templates/helloworld-cloud-cluster/blob/master/application.js#L12-L28). We then [create a function](https://github.com/feedhenry-templates/helloworld-cloud-cluster/blob/master/application.js#L31-L36) which is responsible inside the express application for starting a new server, and call this for every worker.

    
    function startWorker() {
      var port = process.env.FH_PORT || process.env.VCAP_APP_PORT || 8001;
      server = app.listen(port, function(){
        console.log("App started at: " + new Date() + " on port: " + port);
      });
    };


Then, we've got [the same code snippet](https://github.com/feedhenry-templates/helloworld-cloud-cluster/blob/master/application.js#L41-L52) we use across all our cluster-enabled Node apps to startup the master, and then start the subsequent worker nodes using our above startWorker() function:

    
      if (cluster.isMaster) {
        var numCPUs = process.env.FH_NUM_WORKERS || require('os').cpus().length;
        for (var i = 0; i < numCPUs; i++) {
          var worker = cluster.fork();
          workers.push(worker);
        }
      } else {
        startWorker();
      }


If we now run this application, we can see multiple cluster workers have started up:

[![Cluster Workers](/wp-content/uploads/2014/06/cluster-workers-300x158.png)](/wp-content/uploads/2014/06/cluster-workers.png)

My laptop has 8 CPU cores available for Node to use, so we see "App started" console output 8 times. Now, we're going to see cluster in action.
First, let's alter the hello endpoint in lib/hello.js to return the cluster worker ID on every request:

    
    hello.get('/', function(req, res) {
        res.json({worker: require('cluster').worker.workerID});
    });


Now, let's generate some traffic to the /hello endpoint using CURL.

    
    for i in {1..100}; do curl http://localhost:8001/hello; done


[![CURL Output](/wp-content/uploads/2014/06/Screen-Shot-2014-06-10-at-16.55.50.png)](/wp-content/uploads/2014/06/Screen-Shot-2014-06-10-at-16.55.50.png)
In the output, we'll see a whole variety of different workers being returned. This is Node balancing the requests across a number of processes, all of which are running on separate CPUs. We also see if we search for a running `application.js` by doing `ps -a | grep application.js`, we see 9 processes running.

[![application.js processes](/wp-content/uploads/2014/06/Screen-Shot-2014-06-10-at-16.58.48.png)](/wp-content/uploads/2014/06/Screen-Shot-2014-06-10-at-16.58.48.png)
Why are we seeing 9? We have one master process, which distributes the load across these 8 workers - hence 9 processes total.

We've done [some additional work in our workerExitHandler function](https://github.com/feedhenry-templates/helloworld-cloud-cluster/blob/master/application.js#L56-L73) to ensure a clean shutdown of our master, and all the worker nodes too - we bind this function by doing:

    
    cluster.on('exit', workerExitHandler);


You're now cluster ready! Well, almost..


## There's a catch, right?


Of course - there's always a catch! This pattern is great for scaling up applications that serve up a REST API, serving up responses to a client request. The issue is we've effectively introduced 8 instances of our application, with a load balancer in front of it. We've gone from building a single application to having a distributed system on our hands, and we need to manage this. Your application needs to be stateless.

Here's a common pattern to avoid when moving from a single-process application to a clustered application:


### Variables to cache or persist state


JavaScript objects make for a convenient short-term store of data - for example, authentication requests. In the following example, we check for the presence of an api key in the database - if it exists, we set our auth cache to true. Otherwise, it's set to false for this key. Now, future requests will simply return the authCache value of this user's auth.

    
    var authCache = {}, 
    db = require('common/db.js');
    
    exports.checkAuth = function(req, res, next){
      var apikey = req.body.apikey; // Take a param from the request body
      if (authCache[apikey] === true){
        return next();
      }
      db.users.find(username, function(err, authres){ // Some DB helper function we've got
        if (err || !authres){
          authCache[apikey] = false;
          return next('User unauthorised');
        }
        authCache[apikey] = true;
        return next();
      });
    }


Of course, the issue here is our authCache is a variable available just to this cluster worker. All our other cluster workers have a different version of the authCache variable.

The best solution would be to rewrite our code to use a suitable cache, such as redis. This would make our application stateless once again, and stop us checking auth repeatedly.
![Cluster Template](/wp-content/uploads/2014/06/Screen-Shot-2014-06-11-at-15.56.07.png)Of course, cache misses because of a cache that isn't shared across the cluster applications isn't a catostrophic error, but what if we were using the same variable, instead of caching auth results, to store locks on an article that a user was editing?

For more information, the Twelve Factor App's [entry on building Stateless Processes](http://12factor.net/processes) is worth reading.

To Use cluster with a FeedHenry app, just select the 'MBaaS Instance Cluster' template, and your app will have cluster included!


## Conclusion


Cluster is a fantastic way of scaling your application to make use of multiple CPU systems. It does come with some downsides - the API is not yet locked down, and it introduces some of the complexities of a building distributed system. Following best practices should result in any single-process app scaling to use cluster well, however.
The upsides far outweigh the negatives here. It provides a way to scale CPU-intensive operations, and prevent your app from going off the air under load. It should be part of the toolkit of anybody looking to scale their applications.

_[[@cianclarke](http://www.twitter.com/cianclarke) ](/wp-content/uploads/2014/06/Screen-Shot-2014-06-11-at-15.56.07.png)is a Software Engineer with FeedHenry. Primarily focused on the mobile-backend-as-a-service space, Cian is responsible for many of FeedHenry's mBaaS developer features._

  *[mBaaS]: Mobile Backend As A Service
