---
author: cian.clarke
categories:
- Developers
comments: false
date: 2015-09-02T15:33:15Z
link: http://www.feedhenry.com/mobilizing-sharepoint/
slug: mobilizing-sharepoint
title: Mobilizing Sharepoint
url: /mobilizing-sharepoint/
wordpress_id: 5655
---

Microsoft's SharePoint is commonplace in the toolset of a modern organization. It's a silo of information which can be quite difficult to access - but that doesn't always have to be the case.

The Red Hat Mobile Application Platform's SharePoint Connector attempts to solve this problem, by making SharePoint a readily available source of data for mobile developers.

This connector allows you to easily retrieve & create new SharePoint Lists and List Items. The SharePoint REST APIs make it a difficult system to integrate with, and the goal with our new connector is to alleviate some of the [common frustrations](http://cianclarke.com/blog/on-sharepoint-horrible-integrations/) developers face.
For ease of use, the connector can be used with a Service Account to authenticate all requests to SharePoint.
The connector can also authenticate individual users with their SharePoint credentials, providing a persisted session brokered by the Red Hat Mobile Cloud.
Which you use depends on your use case - do want users to provide their SharePoint credentials to access information, or should it be provided as part of another mobile experience that is not necessarily SharePoint centric?

[caption id="attachment_5660" align="alignnone" width="500"]![Discovery UI](/wp-content/uploads/2015/09/Screenshot-2015-09-02-11.08.05-1024x676.png) Sharepoint Connector Discovery Console[/caption]

The connector supports SharePoint 2013 both on-premise and as part of Office 365 online. As with any other Red Hat Mobile connector, all operations are discoverable

In this video, we will see how to create a new SharePoint connector, and retrieve data both using a Service Account and allowing users to authenticate with their own SharePoint credentials.


  *[Service Account]: A system-level account not associated with any specific user, created with the explicit purpose of retrieving information from an API
