---
author: conor.oneill
categories:
- Developers
comments: false
date: 2014-03-05T17:02:20Z
link: http://www.feedhenry.com/dealing-express-4-0-backwards-incompatibilities/
slug: dealing-express-4-0-backwards-incompatibilities
tags:
- express
- node
- npm
title: Dealing with Express 4.0 backwards incompatibilities
url: /dealing-express-4-0-backwards-incompatibilities/
wordpress_id: 3616
---

If you have been running into issues with your Node code today, it is probably due to the new version of Express (4.0) which is now in NPM. In a FeedHenry context you may be seeing that all of your cloud calls respond with the same message - "Your Cloud App is Running".

This will happen if your package.json contains a wildcard "*" for the Express version. So when you deploy your app, it automatically picks the latest version from NPM. Version 4 of Express has a range of backwards compatibility issues which are causing problems.

To deal with this, you should set the Express version number to something like "3.3.4" in that file. In general it is good configuration management practice to specify version numbers for all of the packages you use in your Node code.

As Express 4 is at RC stage, we (and they) do not recommend using it in production. If you wish to know more about what will be involved when it comes time to move to Express 4, you can read the migration guide on the Express [GitHub repo](https://github.com/visionmedia/express/wiki/Migrating-from-3.x-to-4.x). We will provide further updated information on Express 4 when it is ready for production and is in use internally by our own systems.




