---
layout: post
title: "Clustering Issues?"
date: 2012-09-24 14:47
comments: true
categories:
  - computer
  - software
  - clustering
  - troubleshooting
  - ubuntu
  - posts in english
---
The number one culprit for a non working cluster is a misconfigured /etc/hosts file. This is because of how some software implementation announces its availability on the network. Typically, if you have a resolver configured so that the domain name of your local node points to the loopback address, you have a problem.

Some Linux distributions (Ubuntu, I’m looking at you!) ship with an /etc/hosts that looks like this (supposing the hostname is cat and the domain name is foo.bar):

{% codeblock /etc/hosts %}
127.0.0.1    localhost
127.0.1.1    cat.foo.bar cat
{% endcodeblock %}

For some reason, a few software implementations use the resolver to infer the address of the node they’re running on, then start sending out messages such as “Hey, I’m available. You can find me at 127.0.1.1”, which is the problem because you want the node to advertise its availability with a real ip address (usually the one that is bound to the main network interface).

The solution is to get rid of the 127.0.1.1 line. You’re welcome.
