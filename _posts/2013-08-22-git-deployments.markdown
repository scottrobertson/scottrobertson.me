---
title: "Git Deployments"
date: 2013-08-22
layout: post
permalink: /post/git-deployments
categories: git deployment
---

For the past year or so I have been using rsync to deploy my sites, which has worked perfect for me until recently.

I have recently switched to using a simple post-receive hook using a bare repo on the server. I find this works a lot better for me now as it means all I need to do is "push".

First you need to setup the repo on your server, I normally set it up in **/var/git/site.com** for example. This will be a bare git repo, so it will not contain any of your own code. To set this up simply run the following inside your **/var/git/site.com** folder:

~~~
git init --bare
~~~

This will initialize the repository, allowing you to push your code to it.

The next step is to tell git where to check your code out too. I usually use **/var/www/site.com**. For this, I use a very simple post-receive hook that checks the code out to the folder, and runs **composer install**. You would place the following in **/var/git/site.com/hooks/post-receive**:

~~~
WORKING_DIR=/var/www/site.com;
GIT_WORK_TREE=$WORKING_DIR git checkout -f;
cd $WORKING_DIR && composer install -o;
~~~

One thing to remember is to make the file executable by running the the following command:

~~~
chmod +x /var/git/site.com/hooks/post-receive
~~~

Now that the repo and post receive hooks are setup, we need to setup your development machine's git remotes. The easiest way to do this is:

~~~
git remote add production user@site.com:/var/git/site.com
~~~

This will mean that you can simply run the following command to deploy your website:

~~~
git push production master
~~~

You can obviously add anything else you wish to the post-receive hook, such as clearing cache, flushing cdn etc. I currently have most of mine set to restart php-fpm after **composer install**.

~~~
service php5-fpm restart
~~~

If you have any questions, or i have missed anything, then please let me know below.
