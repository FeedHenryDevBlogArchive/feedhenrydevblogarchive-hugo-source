---
author: conor.oneill
categories:
- Developers
comments: false
date: 2015-09-30T16:00:28Z
link: http://www.feedhenry.com/lots-of-lovely-new-features-in-the-red-hat-mobile-application-platform-v3-5-x/
slug: lots-of-lovely-new-features-in-the-red-hat-mobile-application-platform-v3-5-x
title: Lots of lovely new features in the Red Hat Mobile Application Platform v3.5.x
url: /lots-of-lovely-new-features-in-the-red-hat-mobile-application-platform-v3-5-x/
wordpress_id: 5685
---

We have some really interesting and powerful new features in RHMAP v3.5.x which has just been deployed to our evaluation SaaS grids and will be deployed to the production grids next week.



### Lifecycle Management for Forms Builder



We released our Application Lifecycle Management (ALM/LCM) functionality earlier this year and it has proven to be a big hit with customers who need strong lifecycle support in their process. The one area which didn't have this was our Forms Builder and we're pleased to now make LCM available in Forms too.

[![forms_lifecycle_new_02](/wp-content/uploads/2015/09/forms_lifecycle_new_02.png)](/wp-content/uploads/2015/09/forms_lifecycle_new_02.png)



Where previously your Forms went "live" as soon as you saved them, now with LCM you edit a central definition and then deploy your edits to specific Environments. This ensures you can work through your defined lifecycle and not interfere with Production Forms Apps during development. There are also many features available to enable you to deploy, promote and copy Forms in your Environments. You can read more of the details on the [docs site](http://docs.feedhenry.com/v3/guides/app_forms_lifecycle.html).





### Forms Builder support on OpenShift Online



The [Community Edition of RHMAP](https://openshift.feedhenry.com/), which uses OpenShift Online for [its MBaaS](http://docs.feedhenry.com/v3/guides/openshift_online_code_staging.html), is the fastest way to get going with our platform. The most common request we have received since the launch in June, is to have the Forms Builder available there. Our LCM functionality described above now makes that possible.

[![forms5](/wp-content/uploads/2015/09/forms5.png)](/wp-content/uploads/2015/09/forms5.png)



RHMAP + OpenShift Online + Forms Builder is an extremely useful combination that means you can create, build and deploy Cloud-powered Android Apps immediately without writing a line of code. You can read more of the details on the [docs site](http://docs.feedhenry.com/v3/guides/forms_support_openshift.html).



### Revamped Enterprise App Store



Enterprise customers need a way to distribute Apps to their employees. For those who don't require full-blown MDM or MAM, our lightweight Enterprise App Store is ideal. It works for iOS, Android and Windows Apps and isn't limited to Apps you build in the platform.



[![app_store_new](/wp-content/uploads/2015/09/app_store_new.png)](/wp-content/uploads/2015/09/app_store_new.png)

This release sees a much slicker mobile interface for end-users along with more meta-data per App. You can read more of the details on the [docs site](http://docs.feedhenry.com/v3/product_features/admin.html#admin-app_store).



### Windows 8.1 Support including Windows tablets



We have had Windows Phone 8 support since last year and we can now offer support for Windows 8.1 Universal Apps, both Native and Cordova based. The Cordova functionality means that we also support Forms Apps on Windows 8.1 in any form factor.

[![welcome_windows](/wp-content/uploads/2015/09/welcome_windows-1024x799.jpg)](/wp-content/uploads/2015/09/welcome_windows.jpg)

You can read about how Windows is supported [here](http://docs.feedhenry.com/v3/guides/windows_platform_support.html) and how to compile Forms Apps [here](http://docs.feedhenry.com/v3/guides/windows_store_apps_tutorial.html). You can also make use of a Native [Welcome template App](https://github.com/feedhenry-templates/welcome-windows) which will soon be available in the Studio. Expect news on Windows 10 very shortly.



### Cloud App Deployment History



This much-reqested feature shows you all of the specifics of your Cloud App deployments so you know exactly what was deployed to which Environment and when. In addition, you can now navigate away during deployment to do other things and come back whenever it suits you to check the status.

[![deploy_history](/wp-content/uploads/2015/09/deploy_history-1024x629.jpg)](/wp-content/uploads/2015/09/deploy_history.jpg)

We'll be following this soon with v3.6.x which integrates the AeroGear Unified Push Server into RHMAP. All the details here shortly!
