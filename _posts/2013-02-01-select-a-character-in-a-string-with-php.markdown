---
title: "Select A Character in a String with PHP"
date: 2013-02-01
layout: post
permalink: /post/select-a-character-in-a-string-with-php
categories: php
---

I love finding little thing in PHP that i did not know before. They are generally not important, or advanced but it's still very interesting. Here are a few i found recently:

Select a character with a string without using substr etc:

~~~
$string = '1234';
echo $string[1]; // 2
~~~

Another one i found was "__halt_compiler", it's one of the ones i have no real use for but interesting nonetheless. It basically stops the PHP compiler so you can just output HTML from then on without any more server resources being used.

~~~
__halt_compiler();
~~~

I shall no doubt find a lot more of these, and i will have no use for any of them more than likely.
