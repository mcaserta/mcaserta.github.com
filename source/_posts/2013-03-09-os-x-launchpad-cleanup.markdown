---
layout: post
title: "OS X Launchpad Clean Up"
date: 2013-03-09 14:29
comments: true
categories:
  - computer
  - software
  - mac
  - osx
  - launchpad
  - posts in english
---
I quite like OS X <a
href="http://en.wikipedia.org/wiki/Launchpad_(OS_X)">Launchpad</a>. The
only problem is it gets quite messy after a while. The way I fix it is
by spraying napalm over its cache and restarting the dock:

``` bash Clean up Launchpad
$ rm ~/Library/Application\ Support/Dock/*.db && killall -KILL Dock
```

I use this command so often that I have an alias for it:

``` bash Clean up Launchpad shell alias
$ alias culp='rm ~/Library/Application\ Support/Dock/*.db && killall -KILL Dock'
```

The `alias` command should work both in bash and zsh. My mnemonic for
```culp``` is Clean Up Launch Pad.
