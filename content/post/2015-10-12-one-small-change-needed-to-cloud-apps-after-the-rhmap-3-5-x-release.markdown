---
author: conor.oneill
categories:
- Developers
comments: false
date: 2015-10-12T11:59:53Z
link: http://www.feedhenry.com/one-small-change-needed-to-cloud-apps-after-the-rhmap-3-5-x-release/
slug: one-small-change-needed-to-cloud-apps-after-the-rhmap-3-5-x-release
title: One small change needed to Cloud Apps after the RHMAP 3.5.x release
url: /one-small-change-needed-to-cloud-apps-after-the-rhmap-3-5-x-release/
wordpress_id: 5727
---

We have now rolled out RHMAP 3.5.x to all of the relevant pre-production and production grids.

There is one small breaking change which needs to be flagged. If, on deployment of your cloud code, you receive a message saying


    
    "pre-deploy checks failed. Error: fh-mbaas-api version ~x.y.z is not valid. Please make sure it is >=5.0.0"



Then you will need to do the following:




    
  1. Go to the Editor for the Cloud App

    
  2. Click on package.json

    
  3. Locate the line that starts with fh-mbaas-api and change it so the version is "~5.0.0"

    
  4. Click Save

    
  5. You can now re-deploy



If you run into any issues with this change, please contact Red Hat Support via your usual channels.
