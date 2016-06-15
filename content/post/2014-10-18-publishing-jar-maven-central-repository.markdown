---
author: david.martin
categories:
- Blog
- Developers
comments: false
date: 2014-10-18T10:08:51Z
link: http://www.feedhenry.com/publishing-jar-maven-central-repository/
slug: publishing-jar-maven-central-repository
title: Publishing the gitlab-shell Java client to the Maven Central Repository
url: /publishing-jar-maven-central-repository/
wordpress_id: 4939
---

# What library is being published, and why?



The library being published to maven is a java client for interacting remotely with gitlab-shell [https://github.com/gitlabhq/gitlab-shell](https://github.com/gitlabhq/gitlab-shell).
It uses the [Jsch](http://www.jcraft.com/jsch/) libary to remotely connect (SSH) to the machine with gitlab-shell installed, and execute gitlab-shell administration commands.

The reason for creating this library is because one did not exist already. There are many different gitlab client libraries, but none for interacting directly with gitlab-shell, the underlying git repository service for gitlab.



# Creating a new gradle project in Eclipse



To configure Eclipse for working with Gradle projects, the update site at [http://dist.springsource.com/release/TOOLS/gradle](http://dist.springsource.com/release/TOOLS/gradle) was added, and the 'Gradle IDE' plugin installed.
Once installed, a new `Gradle Project' was created based off the 'Java Quickstart' sample project.
The sample project includes the following files & directories by default:


    
    <code>.classpath
    .gradle/
    .project
    .settings/
    bin/
    build/
    build.gradle
    src/main/java/
    src/main/resources/
    src/test/java/
    src/test/resources/
    </code>



The most important files & folders here are:




    
  * .classpath - projcect classpath with a sepcial entry for 'gradle' (org.springsource.ide.eclipse.gradle.classpathcontainer)

    
  * build.gradle - the gradle project build file. A right-click context menu called 'Gradle' will give some useful tasks to run against your project

    
  * src/main/java - your java source files



A sample `.gitignore` file for what should & shouldn't be checked into git can be found here [https://github.com/feedhenry/gitlab-shell-client/blob/master/.gitignore](https://github.com/feedhenry/gitlab-shell-client/blob/master/.gitignore)



# Writing the library



Before starting to write the library, the sample contents in the src folders was removed.
The library is going to be quite simple as it has 2 primary tasks:




    
  * remotely connect to a machine over ssh

    
  * execute a command



To perform the ssh connection, we're using Jsch, a java implementation of the SSH client.
To allow importing it in our source code, we added the following dependency to build.gradle.


    
    <code>dependencies {
        compile 'com.jcraft:jsch:0.1.51'
    }
    </code>



We're focusing on creating and publishing an artifact to maven central in this article, so the library implementation details will be kept brief.
The client follows the Builder pattern, and allows adding, removing & listing both projects & keys.
The java source code for doing this can be found at [https://github.com/feedhenry/gitlab-shell-client/blob/master/src/main/java/com/feedhenry/gitlabshell/GLSClient.java](https://github.com/feedhenry/gitlab-shell-client/blob/master/src/main/java/com/feedhenry/gitlabshell/GLSClient.java).
Usage instructions & sample code can be found in the README file [https://github.com/feedhenry/gitlab-shell-client](https://github.com/feedhenry/gitlab-shell-client)



# Publishing the library



Before the library can be published to maven central [http://search.maven.org/](http://search.maven.org/), we have to publish it to the Sonatype OSS Nexus Repository, then 'Release' it to the central maven repository.
This process is long-winded compared to other package manager repos, such as npm [https://www.npmjs.org/](https://www.npmjs.org/) for node.js pacakges, but becomes easier after the first publication.
For the most part, the guide at [http://central.sonatype.org/pages/ossrh-guide.html](http://central.sonatype.org/pages/ossrh-guide.html) was followed.




    
  * Create a JIRA account at [https://issues.sonatype.org](https://issues.sonatype.org)

    
  * Create a 'New Project' ticket, giving details of the project such as the 'Group ID' (we're using com.feedhenry)

    
  * Wait for a reply, giving details of the repositories that can now be published to and downloaded from

    
  * Publish the release artifact (using gradle) to the staging repository

    
  * Promote the uploaded artifact from 'Staged' to 'Releases'

    
  * Comment back on the JIRA ticket, giving details for the promoted artifact, so that syncronisation of your artifacts to maven central is activated



A example of this ticket process from start to finish can be seen at [https://issues.sonatype.org/browse/OSSRH-11591](https://issues.sonatype.org/browse/OSSRH-11591).
Of all these steps, the hardest is the publishing, as it requires some important configuration before running the publish command.

The first tricky step is configuring your `gradle.properties` file in `~/.gradle/`.
This file contains your username & password for [http://central.sonatype.org](http://central.sonatype.org), and gpg key & password details for signing artifacts before uploading them.


    
    <code>signing.keyId=YourKeyId
    signing.password=YourPublicKeyPassword
    signing.secretKeyRingFile=PathToYourKeyRingFile
    
    ossrhUsername=your-jira-id
    ossrhPassword=your-jira-password
    </code>



The username & password are those from your created account above.
The guide at [http://central.sonatype.org/pages/working-with-pgp-signatures.html](http://central.sonatype.org/pages/working-with-pgp-signatures.html) was followed for setting up a gpg key.
A sample filled in properties file would be:


    
    <code>signing.keyId=171147D9
    signing.password=password
    signing.secretKeyRingFile=/Users/dmartin/.gnupg/secring.gpg
    
    ossrhUsername=david.martin.fh
    ossrhPassword=password
    </code>



The second tricky step is the build.gradle file.
There are a few important configuration blocks needed to tell gradle what we're uploading and where to upload it to.
The guide at [http://central.sonatype.org/pages/gradle.html](http://central.sonatype.org/pages/gradle.html) was very useful in filling out these details.
A full working build.gradle file can be seen at [https://github.com/feedhenry/gitlab-shell-client/blob/master/build.gradle](https://github.com/feedhenry/gitlab-shell-client/blob/master/build.gradle)
The important blocks are:


    
    <code>// artifacts to upload i.e. built archive, javadoc & source code, all in jar format
    artifacts {
      archives javadocJar, sourcesJar
    }
    
    // sign the content before uploading
    signing {
      sign configurations.archives
    }
    
    // upload our artifacts to the staging repository mentioned in the JIRA ticket
    uploadArchives {
      repositories {
        mavenDeployer {
          // ...
    
          repository(url: "https://oss.sonatype.org/service/local/staging/deploy/maven2/") {
            authentication(userName: ossrhUsername, password: ossrhPassword)
          }
    // ...
    </code>



Once all of this is configured, the artifact can be published with:


    
    <code>gradle uploadArchives
    </code>



If the upload was successful, the next step is to 'promote' the artifact from staging to release.
The guide at [http://central.sonatype.org/pages/releasing-the-deployment.html](http://central.sonatype.org/pages/releasing-the-deployment.html) was followed for this step.
This step can be made easier by using the 'Nexus Staging Maven Plugin', but for now we used the website.

Once done, a comment can be left on the JIRA ticket with details of the 'released' artifact.
After some time, there should be a response indicating the maven central sync has been enabled (and may take some time to update)
If everything has gone to plan, you can search for your artifact e.g. [http://search.maven.org/#artifactdetails%7Ccom.feedhenry.gitlabshell%7Cgitlab-shell-client%7C1.0%7Cjar](http://search.maven.org/#artifactdetails%7Ccom.feedhenry.gitlabshell%7Cgitlab-shell-client%7C1.0%7Cjar)



# Using the libary in other projects



The artifact details page on maven central shows Dependency Information for various build tools.
Using the library is a matter of copying & pasting the sample code.
For example, to include our gitlab-shell-client module in a gradle project, we include the following in build.gradle:


    
    <code>dependencies {
        compile 'com.feedhenry.gitlabshell:gitlab-shell-client:1.0'
    }
    </code>



Similarly, to use the artifact in a maven project, we include the following in our pom.xml:


    
    <code><dependency>
        <groupId>com.feedhenry.gitlabshell</groupId>
        <artifactId>gitlab-shell-client</artifactId>
        <version>1.0</version>
    </dependency>
    </code>
