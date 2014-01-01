---
title: "Install Varnish Cache on cPanel Server"
date: 2012-09-29
layout: post
permalink: /post/install-varnish-cache-on-cpanel-server
categories: varnish sysadmin cpanel
---

I have had issues installing [Varnish Cache](http://scottymeuk.in/Joxt) on a cPanel VPS for some time. The main reason is because of the way cPanel deals with the Apache vhost files. It separates them all out into different files for each domain, which makes things very hard when you need to edit them.

Recently, i found a solution for this, and that is the plugin named ApacheBooster. It takes away the pain of installing Varnish on a cPanel server, and makes things a hell of a lot quicker to get setup.

This has helped me reduce the amount of time the images load quite dramatically, and it has also meant that my VPS has more free resources for PHP etc.

To install ApacheBooster on a cPanel server, do the following:

~~~
wget http://prajith.in/downloads/apachebooster.tar.gz
tar -zxf apachebooster.tar.gz
cd apachebooster
sh install.sh
~~~

If you have any questions or problems, please let me know in the comments below.
