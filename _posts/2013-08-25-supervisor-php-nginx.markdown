---
title: "Supervisor+PHP+Nginx"
date: 2013-08-25
layout: post
permalink: /post/supervisor-php-nginx
categories: php nginx mac supervisor
---

While reinstalling my Mac today I realised how easy it was to get PHP and Nginx working with Supervisor. However, if you asked me this the first time i setup my Mac, I probably would have screamed at you for not helping me.

[Supervisor](http://supervisord.org/) allows you to manage any process with ease, and through a simple web ui. You can manage anything from PHP itself, to long running scripts. We use it on [romhut.com](http://romhut.com) to manage the upload workers which are written in PHP, its very powerful to use, once you know.

I figured I'd write a blog post to explain how simple it is, and act as a copy and paste reference for me in the future.

I am going to assume you have the following installing:

 - [Homebrew](http://brew.sh/)
 - PHP (with "--with-fpm" option)
 - Nginx

First we need to install Supervisord, to do this we need to install "pip". You can do this by running the following commands:

~~~
easy_install pip
~~~

This will then allow you to run:

~~~
sudo pip install supervisor
~~~

We can now create our supervisord.conf file, which will tell Supervisor how and what to run. I created mine in `/usr/local/etc/supervisord.conf`. Note that you can create it in a few different locations and Supervisord will automatically pick it up, have a look at their docs for this.

The following is my `supervisord.conf` file, which includes configs to run the web interface:

~~~
[inet_http_server]
port = 127.0.0.1:9001

; Deamon
[supervisord]
logfile = /var/log/supervisord.log
logfile_maxbytes = 50MB
logfile_backups = 5
loglevel = info
pidfile = /tmp/supervisord.pid
nodaemon = false
minfds = 1024
minprocs = 200
user = root
directory = /tmp
strip_ansi = true

; RPC Interface
[rpcinterface:supervisor]
supervisor.rpcinterface_factory  =  supervisor.rpcinterface:make_main_rpcinterface

; CTL
[supervisorctl]
serverurl = http://127.0.0.1:9001

[include]
files = /usr/local/etc/supervisor.d/*.conf
~~~

If you notice on the last line, we are including files from `/usr/local/etc/supervisor.d`. This means we can place config files for PHP, Nginx etc in this folder to keep it cleaner, however you can place all the configs in the above file if you so with.

First off, we will create the config file for php-fpm in `/usr/local/etc/supervisor.d/php.conf`:

~~~
[program:php]
command = /usr/local/sbin/php-fpm
user = root
autostart = true
~~~

The above will run php on startup as root.

Next we will add the Nginx config file in its respective file:

~~~
[program:nginx]
command = /usr/local/bin/nginx
user = root
autostart = true
~~~

This will also run nginx on startup as root. You will also want to add the following to your main nginx config file:

~~~
daemon off;
~~~

This will stop nginx from daemonizing it's self, and is needed for nginx to run smoothly.

Run the following command to tell Supervisor to look for config file changes. Please note that you will need to run this every time you make a change to the config files.

~~~
supervisorctl update
~~~

That should be it all setup ready for you to get these processes running. Simply type the following to test this out:

~~~
supervisorctl start php;
supervisorctl start nginx;
~~~

PHP and Nginx should now be running, to check this simply run. You should see "RUNNING":

~~~
supervisorctl status php
~~~

To get Supervisord to start when your Mac starts, you will want to use launchctl. Simply put the following inside `/Library/LaunchDaemons/com.agendaless.supervisord.plist`:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>KeepAlive</key>
    <dict>
        <key>SuccessfulExit</key>
        <false/>
    </dict>
    <key>Label</key>
    <string>com.agendaless.supervisord</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/supervisord</string>
        <string>-n</string>
        <string>-c</string>
        <string>/usr/local/etc/supervisord.conf</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
{% endhighlight %}

And run:
~~~
launchctl load /Library/LaunchDaemons/com.agendaless.supervisord.plist
~~~

At this stage, Supervisord should be running and ready to use, however I do normally have to restart my Mac for things to work properly. This could have been due to launchctl having not been used before this.

To check that it is running, simply go to <http://127.0.0.1:9001> in your browser, or use the following command:

~~~
supervisorctl status
~~~

If all works well, you should see the following in your browser:

![](http://f.cl.ly/items/1y3C0Q1F1R1C351b1w2z/Screen%20Shot%202013-08-24%20at%2019.38.27.png)

If you have any questions, or suggestions please let me know in the comments below, or [tweet me](http://twitter.com/scottymeuk)
