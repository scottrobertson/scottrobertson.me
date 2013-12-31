---
title: "Copy SSH Keys to Server"
date: 2012-10-12
layout: post
permalink: /post/copy-ssh-keys-to-server
categories: sysadmin
---

When you wish to login to a server without having to enter a password, all you need to do is login with SSH. However I do not know why, but it took me hours to find a simple way to do this a few years ago.

First of all, you need to generate your SSH keys using the following:

~~~
ssh-keygen
~~~

From this point, i generally just hit enter and use the defaults. You can set a password, but that can get annoying.

Now you need to copy the SSH keys to the server you wish to login to. I have found many ways to do this, but none have been as simple as the following:

~~~
ssh-copy-id username@server.com
~~~

You will then be asked for your password for the last time. Once this has complete, you will be able to login to the server using just:

~~~
ssh username@server.com
~~~
