---
author: supittma
categories:
- Developers
comments: false
date: 2016-04-04T13:20:22Z
link: http://www.feedhenry.com/rhmap-google-accounts-android/
slug: rhmap-google-accounts-android
title: RHMAP and Google Accounts in Android
url: /rhmap-google-accounts-android/
wordpress_id: 6018
---

The Red Hat Mobile Application Platform (RHMAP) has strong authentication and authorization mechanisms baked into its [Auth Policy](http://docs.feedhenry.com/v3/product_features/admin.html#admin-auth_policies) system. Android has deep integration with Google's ecosystem which provides many easy mechanisms for authorizing services to act on a user's behalf. Out of the box RHMAP allows for connecting to a Google account using OAuth and a web view, but a better user experience is using Google's Android account picker. To enable this integration in RHMAP we can use an MBaaS Auth Policy.



# Prerequisites



This post should be informative to anybody who wishes to learn more about RHMAP; however, you will have the most benefit if you have access to a RHMAP instance and have read through the [Getting Started](http://docs.feedhenry.com/v3/getting_started.html) documentation. If you do not have access to a instance of RHMAP, you may sign up for a free one at [openshift.feedhenry.com](https://openshift.feedhenry.com/).

Additionally you will need a Google account and Android emulator or device with Google's APIs set up.



# Demo



You can view an example of this integration in my [FehBot video](https://www.youtube.com/watch?v=eNLKj6wvbQg). The Android portion of this post will refer to the code in the application.



# Creating an MBaaS Auth Policy





## Create a blank MBaaS Service



Select "Services & APIs" from the top navigation. Click "Provision MBaaS Services/API"

[![CReate_MBaaS_1](/wp-content/uploads/2016/04/CReate_MBaaS_1-300x121.png)](/wp-content/uploads/2016/04/CReate_MBaaS_1.png)

Select "Choose" next to the item "New mBaaS Service".

[![CReate_MBaaS_2](/wp-content/uploads/2016/04/CReate_MBaaS_2-300x121.png)](/wp-content/uploads/2016/04/CReate_MBaaS_2.png)

Name the service, click "Next", ensure you are using the "Development" environment, and finally click "Deploy". The service should deploy and you should have a green bar.

[![CReate_MBaaS_3](/wp-content/uploads/2016/04/CReate_MBaaS_3-300x121.png)](/wp-content/uploads/2016/04/CReate_MBaaS_3.png)

You are now ready to set up the Auth Policy.



## Setup the Auth Policy



Select "Admin" from the top navigation and then "Auth Policies" from the 6 boxes which appear.

[![Create_auth_policy_1](/wp-content/uploads/2016/04/Create_auth_policy_11-300x121.png)](/wp-content/uploads/2016/04/Create_auth_policy_11.png)

Click "Create" on the next screen to begin setting up an Auth Policy.

[![CReate_Auth_Policy_2](/wp-content/uploads/2016/04/CReate_Auth_Policy_2-300x121.png)](/wp-content/uploads/2016/04/CReate_Auth_Policy_2.png)

Name the Policy and select "MBaaS Service" as the "Type" under "Authentication. From the "Service" drop down select the service you created in the previous step. For "Endpoint" our MBaaS service will use "/auth/init". Finally select for your "Default Environment" the value "Development".

[![CReate_Auth_Policy_3](/wp-content/uploads/2016/04/CReate_Auth_Policy_3-300x121.png)](/wp-content/uploads/2016/04/CReate_Auth_Policy_3.png)

Scroll down to the bottom of the page and click "Create Auth Policy".



# Implementing the MBaaS



I have created a [MBaaS Service](https://github.com/secondsun/FH-Google-mBaas-Auth) for us to use. It implements the server side token validation that Google recommends in [its documentation](https://developers.google.com/identity/sign-in/android/backend-auth). You should be able to copy this project into your MBaaS's source and redeploy it.

You may wish to limit which Cloud applications can access your MBaaS services in the "Service Settings" section of the MBaaS "Details" page.

[![Create_MBaas_4](/wp-content/uploads/2016/04/Create_MBaas_4-300x121.png)](/wp-content/uploads/2016/04/Create_MBaas_4.png)



## /auth/init



The [/auth/init](https://github.com/secondsun/FH-Google-mBaas-Auth/blob/master/lib/auth.js#L12) route will consume tokens from the Android device and set up user accounts in RHMAP. The code should be easy ish to follow along. The most important part is that we return a [userId](https://github.com/secondsun/FH-Google-mBaas-Auth/blob/master/lib/auth.js#L25) value in the json which we can use to look up the user's session informaiton.



## /list/:session



The route [/list/:session](https://github.com/secondsun/FH-Google-mBaas-Auth/blob/master/application.js#L23) can be used by Cloud applications to fetch a user's account information which is created and saved after a call to "/auth/init".



# Android Integration



In order to integrate with Android, please follow [Google's Guide](https://developers.google.com/identity/sign-in/android/sign-in#configure_google_sign-in_and_the_googleapiclient_object) for instructions on how to setup an Android account and get an IdToken from a sign in. The [FehBot Android client](https://github.com/secondsun/fehbot-android/blob/master/app/src/main/java/com/feedhenry/oauth/oauth_android_app/) contains a working example.

Once you have a IdToken you can use [FH.buildAuthRequest](https://github.com/feedhenry/fh-android-sdk/blob/master/fh-android-sdk/src/main/java/com/feedhenry/sdk/FH.java#L279) to perform the sign-in with RHMAP. For the three parameters us the Auth Policy name you assigned during "Setup the Auth Policy", the IdToken you retrived from Google, and an empty string for the final parameter. [Here](https://github.com/secondsun/fehbot-android/blob/master/app/src/main/java/com/feedhenry/oauth/oauth_android_app/FHOAuth.java#L127) is an example from the FeHBot app.



# Caveats



As per the [RHMAP Authentication API](http://docs.feedhenry.com/v3/api/api_auth.html#api_auth-app_api-create_your_own_authentication_providers) if you use this you will have to manually verify your sessions in your application yourself. The built in verification methods will not work.



# Conclusion



As you can see, it is easy to add a third party authentication mechanism to RHMAP. The principles in this post can be applied to many other authentication providers and client platforms.
