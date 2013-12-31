---
title: "Dropbox Server Backups"
date: 2013-08-24
layout: post
permalink: /post/dropbox-server-backups
categories: php dropbox
---

Over the past few months I have been searching for a backup solution for my VPS's. I wanted a single solution that allowed me to backup multiple sources such as MongoDB, MySQL and the usual files.

After a while searching, I decided I would build it myself, due to not being able to find the perfect solution.

Before this, I simply had the Dropbox service running on each machine, and had a cron that copied mysqldump into the Dropbox folder, which was synced. This worked well, but it was quite laborious to setup.

Because of this, I opted to use the Dropbox API directly to backup from the tool. This allowed me to deploy the script to any server ([see here](/post/git-deployments)) and the only dependencies are PHP and Curl.

When setting it up, all you need to do is run:

~~~
bin/console dropbox:auth
~~~

This will give you an authorisation link for Dropbox, which will return a token which is used to build your config file.

Once you have set the config file up (see the repo's README.md for details). You will be able to simply setup a cron to run the following for instance:

~~~
bin/console export:mysql
~~~

This will loop over each database you have access to, mysqldump them, gzip them and upload them to Dropbox. My MySQL (and MongoDB) backups, are set to backup once per hour, but the file is stored YMD, which means that the file will be overwritten. This would normally be a bad thing since you could overwrite with a bad backup easily, but because we are using Dropbox, it keeps internal revisions meaning that we have around 23 copied per day, but we keep the storage space to a minimum.

If you would like to view the source, and maybe use the tool, then please head over to [Github](http://github.com/scottrobertson/backup). Let me know if you have any questions, or suggestions.

