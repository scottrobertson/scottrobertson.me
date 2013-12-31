---
title: "Google's Mod_Pagespeed"
date: 2012-10-23
layout: post
permalink: /post/googles-modpagespeed
categories: sysadmin
---

"mod_pagespeed speeds up your site and reduces page load time. This open-source Apache HTTP server module automatically applies web performance best practices to pages, and associated assets (CSS, JavaScript, images) without requiring that you modify your existing content or workflow."

When I was in the process of setting up my new VPS, I decided I would try out mod_pagespeed (conveniently it had just came out of beta a few days earlier). It turns out, its very easy to setup and very easy to use.

First of all, you need to download the correct package over at:

~~~
https://developers.google.com/speed/docs/mod_pagespeed/download
~~~

Once you have downloaded it, its as simple as running:

~~~
dpkg -i mod-pagespeed-*.deb
apt-get -f install
Or for CentOS you run
yum install at  # if you do not already have 'at' installed
rpm -U mod-pagespeed-*.rpm
~~~

The default install includes things such as the minification of css and js files and also the combining of css files (js can be enabled too). Inline images, css, and js is also enabled by default, which allows these types of files to be included in the main HTML layout if they are small enough to justify doing so. This will save requests to the server for more files.

As well as the defaults, i decided to enable a few none default options in the config file:

~~~
/etc/apache2/mods-available/pagespeed.conf
The ones i decided to enable where the following:
ModPagespeedEnableFilters inline_preview_images     # Load low quality images before loading high quality ones
ModPagespeedEnableFilters resize_images             # Resize the images before serving them (These are then cached in Varnish)
ModPagespeedEnableFilters insert_image_dimensions   # If any image does not have width, height set, then insert them
ModPagespeedEnableFilters combine_javascript        # Combine javascript files into one file
ModPagespeedEnableFilters collapse_whitespace       # Minify the HTML output of the page
ModPagespeedEnableFilters resize_mobile_images      # Resize smaller images on mobile
ModPagespeedEnableFilters remove_comments           # Remove comments from JS,CSS and HTML files.
~~~

Some are not 100% ready for production but the risk value of them is quite low.

From initial tests, it seems to have reduced the page load by about 1/3. Which on a small site may not be a lot, but on a big site with a lot of images etc, this could be a huge saving in bandwidth costs, and also a lot nicer experience for the user.
