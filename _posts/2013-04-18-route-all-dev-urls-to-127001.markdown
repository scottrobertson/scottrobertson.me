---
title: "Route all .dev URL's to 127.0.0.1"
date: 2013-04-18
layout: post
permalink: /post/route-all-dev-urls-to-127001
categories: development
---

If you're like me, you always forget to edit the /etc/hosts file when you want to view a local site.

To get around this, you can use the following script to route all your .dev urls to 127.0.0.1 (or any other IP). So you could go to "romhut.dev" and that would load from 127.0.0.1.

~~~
brew install dnsmasq
mkdir -pv $(brew --prefix)/etc/
echo 'address=/.dev/127.0.0.1' > $(brew --prefix)/etc/dnsmasq.conf
sudo cp -v $(brew --prefix dnsmasq)/homebrew.mxcl.dnsmasq.plist /Library/LaunchDaemons
sudo launchctl load -w /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
sudo mkdir -v /etc/resolver
sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/dev'
~~~

If you wish to use another IP, only change the first 127.0.0.1, otherwise it will look for a name server on the host.
