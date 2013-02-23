---
layout: post
title: "SBT Company Wide Settings Example"
date: 2013-02-23 14:29
comments: true
categories:
  - computer
  - software
  - scala
  - sbt
  - posts in english
---
I know it looks like I haven't been posting much lately. Anyway, I'm studying <a href="http://www.scala-lang.org/">Scala</a> programming and, among other things, I came upon the awesome <a href="http://www.scala-sbt.org/">Simple Build Tool</a> (SBT for short).

SBT looks like a mighty and untamable beast at first glance. But, after a closer look and a patient walkthrough of the <a href="http://www.scala-sbt.org/release/docs/index.html">online docs</a> and the practical examples, I must say it is an awesome tool, very worth the learning effort. 

One of the first issues I had to face in SBT was implementing what I was previously doing with a <a href="http://code.google.com/p/m4enterprise/wiki/CorporatePOM">Maven Corporate POM</a>. In other words, I needed a mechanism to control the master build settings for all of a company's artifacts.

I ended up publishing an <a href="https://github.com/mcaserta/sbt-bom-example" title="SBT Company Wide Settings Example">SBT Company Wide Settings Example project on GitHub</a>. There is an extensive <a href="https://github.com/mcaserta/sbt-bom-example/blob/master/README.markdown">readme</a> that explains how I am doing it.

As usual, feedback is welcome.
