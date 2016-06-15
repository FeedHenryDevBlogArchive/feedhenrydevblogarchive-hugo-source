---
author: brian.dooley
categories:
- Developers
comments: false
date: 2014-04-04T13:43:12Z
link: http://www.feedhenry.com/getting-started-mysql-feedhenry-studio/
slug: getting-started-mysql-feedhenry-studio
tags:
- amazon
- AWS
- mysql
- plugin
title: Getting started with MySQL and the FeedHenry studio.
url: /getting-started-mysql-feedhenry-studio/
wordpress_id: 3705
---

# Introduction


The FeedHenry platform allows apps to connect to external MySQL instances.  The following is a run-through of the process required to create a MYSQL instance within the Amazon cloud and creating an app to pull data from it. This document contains many screenshots of the process as a visual aid to the procedure but it is possible that websites outside of FeedHenrys control may change and not match the screenshots in the future. For the purposes of this procedure, we are going to create a micro RDS instance of MySQL on the Amazon cloud.  The MySQL instance documented here is for demonstration purposes.  There is currently no charge by Amazon for running this instance but that could possibly change.


# Method




## Part 1 - Amazon


The first requirement for this task is to create your MySQL instance within the cloud.  In this case, we will use Amazon's RDS facility. The first step is to set yourself up with an Amazon AWS account.  Instructions on how to do this can be found at [http://aws.amazon.com/](http://aws.amazon.com/) by clicking "Sign Up".  Follow the on screen instructions to create your Amazon AWS account. Next, making sure you are logged into your new account, go to [https://console.aws.amazon.com/console/home?#](https://console.aws.amazon.com/console/home?#) and select RDS .....

[![Screenshot 2014-03-20 15.34.40](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.34.40.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.34.40.png)

.... which will take you to the RDS console where you can keep track of all your database instances.

To begin creating an instance of MySQL, click on the _Launch a DB Instance_ button.

[![Screenshot 2014-03-20 15.37.29](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.37.29.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.37.29.png)

Once here, we are presented with a selection of databases to choose from.  Select MySQL.

[![Screenshot 2014-03-20 15.41.01](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.41.01.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.41.01.png)

For our purposes, we will use RDS free usage.  Select that option and select _Next Step_.

[![Screenshot 2014-03-20 15.47.55](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.47.55.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.47.55.png)

We will stick with the defaults here.  However, there are a few field that need to be filled in by the user.  For this demonstration, we will keep things small.  Select the following values ...



	
  * Db Instance Class is _db.t1.micro_.

	
  * Multi-AX Deployment is _No_.

	
  * Allocated Storage is the minimum _5Gb_.

	
  * Db Instance Identifier is the name you wish to give to your MySQL instance.  In this case, we will go with _myTestInstance_.

	
  * Master Username and Master Password are up to you.  Just make sure to write them down after you decide them.


[![Screenshot 2014-03-20 15.54.42](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.54.42.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-15.54.42.png)

Click _Next Step_

__ [![Screenshot 2014-03-20 20.21.00](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.21.00.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.21.00.png)

**Note that this document assumes that you are using a brand new, just created, AWS account.  A new account will provide the correct default values for VPC settings shown here but for older accounts, the default values may be different.  There may not be a _Choose a VPC_ option available, for example.  You can find details regarding the differences in VPC for older accounts at [http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html).  **

Pick a Database name (in this case, I have picked _testDb1_).  Leave the rest of the fields with their default values and click _Next Step._

[![Screenshot 2014-03-20 20.24.47](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.24.47.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.24.47.png)

Keep the defaults and select _Next Step._

[![Screenshot 2014-03-20 16.56.11](/wp-content/uploads/2014/03/Screenshot-2014-03-20-16.56.11.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-16.56.11.png)

Final step, for now.  Select _Launch Db Instance_. The DB Instance takes a few minutes to create.  You can view the progress of the instance creation by clicking on the _View your DB Instances on the DB Instances page_ link.

[![Screenshot 2014-03-20 16.58.34](/wp-content/uploads/2014/03/Screenshot-2014-03-20-16.58.34.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-16.58.34.png)

Eventually, the instance is up and running.  Click on the instance in the _DB Instances_ page to see its details.


### Security


The instance you have just created is up and running.  However, we must ensure that the security group the instance is running with will allow MySQL connections, otherwise connecting our app to this instance will be impossible.

[![Screenshot 2014-03-20 20.39.35](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.39.35.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.39.35.png)

Click on the link beside _Security Groups_ within your instance and select the _Inbound_ tab

[![Screenshot 2014-03-20 20.49.11](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.49.11.png)](/wp-content/uploads/2014/03/Screenshot-2014-03-20-20.49.11.png)



Add a new rule consisting of ....
<table align="left" border="0" >
<tbody >
<tr >

<td >**Type**
</td>

<td >**Protocol**
</td>

<td >**Port Range**
</td>

<td >**Source**
</td>
</tr>
<tr >

<td >MYSQL
</td>

<td >TCP
</td>

<td >3306
</td>

<td >0.0.0.0/0
</td>
</tr>
</tbody>
</table>




... to allow access from a MySQL client/app.



Part 2 - Creating your app.

Almost there!  Final stage is to create an app that can access the instance of RDS that you have created.  Within the studio, select "Plugins" ....

[![Screenshot 2014-04-03 12.10.50](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.10.502.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.10.502.png)

... and then select "MySQL".

[![Screenshot 2014-04-03 12.13.49](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.13.49.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.13.49.png)



The current instructions for adding the MySQL plugin will be displayed.

[![Screenshot 2014-04-03 12.16.05](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.16.05.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.16.05.png)



In order to get you started faster, here's an app we rustled up earlier that you can use to access your MySQL instance!  You can find it at [https://github.com/FeedHenrySuprt/fh_mysql](https://github.com/FeedHenrySuprt/fh_mysql).

Perform the following ...

First, login into your GitHub account (if you do not have a GitHub account, you can set one up at [https://github.com/](https://github.com/)).  Once logged in, go to [https://github.com/FeedHenrySuprt/fh_mysql](https://github.com/FeedHenrySuprt/fh_mysql) and fork this repository to your own.

Then, in a 2nd browser tab, log into your FeedHenry domain.

Under the _Apps_ tab, press _Create an App_

[![Screenshot 2014-04-03 12.21.01](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.21.01.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.21.01.png)



On the pop up screen, select _Create an App from a Git Repository_ and click _Next_.

[![Screenshot 2014-04-03 12.22.36](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.22.36.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.22.36.png)

Give your app a name, use the "SSH Clone URL" of the forked app as your _Git URL_ and enter _Master_ for the branch.  Click _Next_

[![Screenshot 2014-04-03 12.24.27](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.24.27.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.24.27.png)

The next screen gives you a public key to add to your git setup.

[![Screenshot 2014-04-03 12.32.32](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.32.32.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.32.32.png)

You add this to your github account by copying the entire key to your clipboard, going to [https://github.com/settings/ssh](https://github.com/settings/ssh) and adding the key.  Once added, click _Next_ and your app will be created.

[![Screenshot 2014-04-03 12.58.02](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.58.02.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-12.58.02.png)

Click _Finish_.

Soon be finished.  The FeedHenry studio has a facility to create _Environment Variables_ that you can place the details of your MySQL instance into that can be accessed by the app.  This is quite a handy facility as it means you can change the database that the app points to fairly easily.



On the sidebar, select _Environment Variables_

[![Screenshot 2014-04-03 17.14.13](/wp-content/uploads/2014/04/Screenshot-2014-04-03-17.14.13.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-17.14.13.png)



... and create the following variable and push them, to both Dev and Live.

MYSQL_HOST - This is the endpoint for your instance.  You can get the value for this from the AWS display for your RDS instance.

[![scr](/wp-content/uploads/2014/04/scr.png)](/wp-content/uploads/2014/04/scr.png)



MYSQL_USERNAME - Username for your instance.

MYSQL_PASSWORD - Password for your instance.

Don't forget to push your variables once they have been created.

Once that is done, you are ready to go.  Go to the _Details_ section of your studio and click on the _Emulator_ at the right side of the screen ...

[![Screenshot 2014-04-03 17.27.31](/wp-content/uploads/2014/04/Screenshot-2014-04-03-17.27.31.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-03-17.27.31.png)



to spawn an emulator window for the app.

You can enter SQL command into the white text field and view the returned results in the green field.  Try the following to get started (make sure you use the correct database name when making your SQL queries as specified in the _Additional Config_ section in the AWS setup process), make sure you hit _Submit_ after each line and watch the results in the green section.

[code]

create table testDb1.myStudents (student_number int, firstname varchar(30), surname varchar(30));

insert into testDb1.students (student_number, firstname, surname) values (1, 'Joe', 'Bloggs');
insert into testDb1.students (student_number, firstname, surname) values (2, 'Jane', 'Brown');
insert into testDb1.students (student_number, firstname, surname) values (3, 'Mick', 'Jones');

select * from testDb1.students;

[/code]

[![Screenshot 2014-04-04 08.47.04](/wp-content/uploads/2014/04/Screenshot-2014-04-04-08.47.04.png)](/wp-content/uploads/2014/04/Screenshot-2014-04-04-08.47.04.png)





Final Thoughts.

Now that you have basic app that can access a MySQL database running on the cloud, the stage is set for you to integrate all MySQL functionality into your FeedHenry apps from now on.  You can find more information about using the FeedHenry MySQL plugin [here](https://github.com/felixge/node-mysql) as well as more information about [getting started with AWS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html) as well.


