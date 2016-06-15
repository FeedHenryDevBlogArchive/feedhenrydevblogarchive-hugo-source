---
author: david.martin
categories:
- Blog
- Developers
comments: false
date: 2014-10-11T12:25:34Z
link: http://www.feedhenry.com/gitlab-shell-chef-cookbook/
slug: gitlab-shell-chef-cookbook
tags:
- automation
- chef
- cookbook
- git
- gitlab
- gitlab-shell
- ruby
title: Creating & Publishing a Chef Cookbook for gitlab-shell
url: /gitlab-shell-chef-cookbook/
wordpress_id: 4932
---

# What was done and why?



This article details the steps that were followed to:




    
  * Create a new chef cookbook (for installing gitlab-shell)

    
  * Write a recipe, with a set of configurable attributes

    
  * Deploy/Publish this cookbook to the 'Chef Supermarket'

    
  * Include the newly published cookbook in chef based project



The main reason for doing this was because a cookbook for installing **just** gitlab-shell did not exist (More than 1 for a full gitlab installation did)



# Creating the cookbook



The `knife` command is the main cli tool for doing chef stuff. For example, to create a new cookbook in the default cookbook location, do this:


    
    <code>knife cookbook create <coobookname>
    </code>



The default location is usually `~/.chef/cookbooks/`. To specify a location, the `-o` switch can be used e.g.


    
    <code>knife cookbook create gitlab-shell -o ~/work
    </code>



This creates a bunch of files & folders, a lot of which won't be needed this time. Some files will have useful placeholder info, especially the README.


    
    <code>-rw-r--r-- CHANGELOG.md
    -rw-r--r-- README.md
    drwxr-xr-x attributes
    drwxr-xr-x definitions
    drwxr-xr-x files
    drwxr-xr-x libraries
    -rw-r--r-- metadata.rb
    drwxr-xr-x providers
    drwxr-xr-x recipes
    drwxr-xr-x resources
    drwxr-xr-x templates
    </code>



The main files & folders are:




    
  * metadata.rb - cookbook definition & version

    
  * README.md - cookbook details, usage and attributes/config information

    
  * CHANGELOG.md - your cookbook changes for each version

    
  * /attributes - configuration files go in here

    
  * /recipes - installation & setup script(s)

    
  * /files - files to 'drop in' during installation and setup

    
  * /templates - files to 'drop in' with placeholders replaced with config values





# Cookbook recipe



This is a simple recipe, so we can do everything in a single file to install & setup gitlab-shell. This file is `recipes/default.rb` To install gitlab-shell, there are a few steps




    
  * Create a new user called `git`

    
  * Clone the gitlab-shell repo from github

    
  * Create a config.yml file using a chef template file

    
  * Ensure the gitlab repositories path is created and has the correct perms

    
  * Ensure the `git` users authorized_keys file is created with the correct perms



The complete source for this can be found at [https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/recipes/default.rb](https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/recipes/default.rb) The most interesting part of the recipe is the template file for config.yml. The template file is in templates/default/gitlab_shell.yml.erb [https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/templates/default/gitlab_shell.yml.erb](https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/templates/default/gitlab_shell.yml.erb) It's an embedded ruby template file, so there are some ruby placeholders in it. For example, the url to call back to for checking repo access and key permissions is injected here:


    
    <code>gitlab_url: <%= @url %>
    </code>



Back in the recipe, the template file is output to the gitlab-shell install directory as config.yml. The code to do this looks like:


    
    <code>template File.join(gitlab_shell['shell_path'], "config.yml") do
      source "gitlab_shell.yml.erb"
      user gitlab_shell['user']
      group gitlab_shell['group']
      variables({
        # ....
        :url => gitlab_shell['url'],
        # ....
      })
    end
    </code>



The variables section is the object/hash passed into the template file. The value for this config is obtained from the cookbook attributes. More specifically, the file at attributes/default.rb [https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/attributes/default.rb](https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/attributes/default.rb) The `gitlab_url` value comes from the following attribute:


    
    <code>default['gitlab-shell']['url'] = "http://localhost:3000/"
    </code>





# Cookbook preparation for publish



Now that we have a cookbook, we can publish it to the Chef Supermarked at [https://supermarket.getchef.com](https://supermarket.getchef.com). To do this, we need an account. At the time of writing, and account can be created at [https://manage.opscode.com/signup?ref=community](https://manage.opscode.com/signup?ref=community). After creating one and signing in, you'll be prompted to download/save your newly generated private key. The suggested location is `~/.chef/<userid>.pem`. This can be used later when we want to upload our cookbook from the terminal. Before uploading, we should make sure our README.md and metadata.rb files are in order.



## README.md



[https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/README.md](https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/README.md) This file is whats shown to users browsing cookbooks in the Chef Supermarket. The suggested content, based on the generated file form the `knife` command earlier, is:




    
  * Requirements - e.g. other cookbook dependencies, platforms supported

    
  * Attributes - List (or better yet, a table) of attributes you cookbook uses, with their default values

    
  * Usage - Which recipe to run - usually ::default.rb

    
  * Contributing Guide - if you cookbook is open source/public

    
  * License & Authors





## metadata.rb



[https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/metadata.rb](https://github.com/feedhenry-cookbooks/gitlab-shell/blob/master/metadata.rb) This is the most imporant file when uploading your cookbook. It contains the cookbook name & version number, among other things:


    
    <code>name             'gitlab-shell'
    maintainer       'FeedHenry'
    maintainer_email 'david.martin@feedhenry.com'
    license          'All rights reserved'
    description      'Installs/Configures gitlab-shell'
    long_description IO.read(File.join(File.dirname(__FILE__), 'README.md'))
    version          '0.3.0'
    </code>



Ever time we want to publish our cookbook, we should bump the version number. This ensures we keep a history of the coookbook and don't break it for users of previous versions. When we're happy that everything is in order & ready to go, the publish command is:


    
    <code>knife cookbook site share -k ~/.chef/<userid>.pem "<cookbook_name>" "<category>" -u <userid> -VV -o <cookbook_location>
    </code>



For example:


    
    <code>knife cookbook site share -k ~/.chef/davidmartin.pem "gitlab-shell" "Other" -u davidmartin -VV -o ~/work
    </code>



More details on the `knife cookbook site` command, including available categories, can be found here [https://docs.getchef.com/knife_cookbook_site.html](https://docs.getchef.com/knife_cookbook_site.html) After publishing, we can find our cookbook by searching on the Chef Supermarket [https://supermarket.getchef.com/cookbooks](https://supermarket.getchef.com/cookbooks) The finished product for this article can be found at [https://supermarket.getchef.com/cookbooks/gitlab-shell](https://supermarket.getchef.com/cookbooks/gitlab-shell)



# Using the cookbook



Using the cookbook is simple. In your Chef project, add the following line to your Chefile


    
    <code>cookbook 'gitlab-shell'
    </code>



and run


    
    <code>librarian-chef install
    </code>



after which you should see the cookbook installed into cookbooks/gitlab-shell in your project.



# Credit



Credit goes to the maintainers of the gitlab cookbook at [https://gitlab.com/gitlab-org/cookbook-gitlab/tree/master](https://gitlab.com/gitlab-org/cookbook-gitlab/tree/master). This gitlab-shell cookbook is a subset of the gitlab cookbook.
