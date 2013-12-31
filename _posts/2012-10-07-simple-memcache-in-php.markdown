---
title: "Simple Memcache In PHP"
date: 2012-10-07
layout: post
permalink: /post/simple-memcache-in-php
categories: php memcached
---

In the past year or so i have been using caching more and more. It has become a massive part of my job, and pretty much the whole site depends on it, and without it things would start to fall over very quickly.

Here is a basic example of using Memcache in PHP:

{% highlight php linenos %}
<?php
$cache = new Memcache;
$cache->connect('127.0.0.1', 11211);

function getPosts()
{
    $cache_key = 'getPosts';
    $posts = $cache->get($cache_key);

    if ($posts === false)
    {
        $posts = DB::query('SELECT * FROM posts');
        $cache->set($cache_key, $posts);
    }

    return $posts;
}
{% endhighlight %}
