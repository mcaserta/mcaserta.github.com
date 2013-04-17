---
layout: post
title: "An Introduction to Time Representation, Serialization and Management in Software"
date: 2013-04-15 14:19
comments: true
categories:
  - computer
  - software
  - software development
  - time
  - posts in english
---
Most issues in software development usually arise from poor,
inconsistent knowledge of the domain at hand. A topic apparently as
simple as time representation, serialization and management can easily
cause a number of problems both to the neophyte and to the experienced
programmer.

In this post, we'll see that there's no need to be a 
[Time Lord](http://en.wikipedia.org/wiki/Time_Lord) to grasp the
very simple few concepts needed not to incur into time management hell.

{% img /images/blog/time.jpeg 500 341 %}

## Representation

A question as simple as *"What time is it?"* underlies a number of
contextual subleties that are obvious to the human brain, but become
absolute nonsense for a computer.

For instance, if you were asking to me the above question right now, I
might say: *"It's 3:39"* and, if you were a colleague in my office,
that'd be enough information to infer that it's 3:39pm CEST. That's
because you would already be in possession of some bits of important
contextual information such as 

* it's an afternoon because we've already had lunch
* we're in Rome, therefore our timezone is Central European Time (CET)
  or Central European Summer Time (CEST)
* we've switched to daylight savings time a few weeks earlier so, the
  current timezone must be Central European Summer Time

*3:39* only happens to be a convenient representation of time as long as
we're in possession of the contextual bits.  In order to represent time
in an universal way, you should have an idea what
[UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time) and
[timezones](http://en.wikipedia.org/wiki/Time_zone) are. 

Now, suppose I have to schedule a skype chat with a fellow software
developer in the US. I could write him an email and say something along
the lines of *"see you on 2/3"*. In Italy, that would be the second day
of the month of march, but to an US person, that would be the third day
of the month of february. As you can see, how our chat is never going to
happen.

These are only a few examples of the kind of issues that might arise
when representing date and time information. Luckily enough, there is a
solution to the representation conundrums, namely the 
[ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) standard.

Just to give you an example, in ISO 8601, `1994-11-05T08:15:30-05:00`
corresponds to November 5, 1994, 8:15:30 am, US Eastern Standard Time.
`1994-11-05T13:15:30Z` corresponds to the same instant (the `Z` stands
for UTC). Same instant, different representations.

The ISO 8601 standard also has the nice side effect of providing natural
sorting in systems that use lexicographical order (such as filesystems)
because information is organized from most to least significant, i.e.
year, month, day, hour, minute, second, fraction of second.

Even if you're only dealing with local times in your software, you
should know that, unless you also display the time zone, you can never
be sure of the time. I cannot remember how many times a developer has
asked me to *fix the time* on the server, only to discover that his
software was printing time in UTC.

At display time, it is okay to deal with partial representation of time
because the user experience requires so. Just make sure, when debugging,
to print out the whole set of information, including the time zone,
otherwise you can never be sure what you're looking at is what you
actually think it is.

Although a given moment in time is immutable, there is an arbitrary
number of ways to express it. And we've not even talked about the Julian or
Indian calendars or stuff like expressing durations!

Let me summarize a few key points to bring home so far:

* get to know [time zones](http://en.wikipedia.org/wiki/Time_zone) and
  [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time)
* [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) is your friend
* always print the time zone while debugging

## Serialization

Speaking of software, serialization is a process where you take an
entity and spell it out in such a way that it can be later entirely
rebuilt, exactly like the original, by using the spelt out (serialized)
information.

In the binary world of computers, time is usually serialized and stored
by using the [Unix time](http://en.wikipedia.org/wiki/Unix_time)
convention. As I'm writing this, my Unix time is `1366191727 UTC`. That
is: `1366191727` seconds have passed since January 1st, 1970 at 00:00
UTC. Isn't that a pretty clever, consistent and compact way of
representing a plethora of information, such as `April 17 2013 @
11:42:07am CEST`?

Unix time is only another arbitrary representation of a given moment in
time, although a not very human readable one. But you can take that
number, write it on a piece of paper, stick it to a carrier pigeon, and
your recipient would be able to decipher your vital message by simply
turning to the Internet and visiting a site such as
[unixtimestamp.com](http://www.unixtimestamp.com/).

Just like you can write that number on a piece of paper and later get
back the full instant back to life, you can store it in a file or a
row in your favorite RDMBS. Although you might want to talk to your
RDBMS using a proper driver and handing it a plain date instance; your
driver will then take care of the conversion to the underlying database
serialization format for native time instances. 

By storing time using a native format, you get for free the nice
time formatting, sorting, querying, etc features of your RDBMS, so you
might want to think twice before storing plain Unix timestamps.

Just make sure you know what timezone your Unix timestamp refers to, or
you might get confused later at deserialization time.

ISO 8601 is also a serialization favorite. In fact, it is used in the
[XML Schema](http://www.w3.org/TR/xmlschema-2/#isoformats) standard.
Most xml frameworks are natively able to serialize and deserialize back
and forth from `xs:date`, `xs:time` and `xs:dateTime` to your
programming language's native format (and viceversa). Just be careful
when dealing with partial representations: for instance, if you omit the
time zone, make sure you agree beforehand on a default one with your
communicating party (usually UTC or your local time zone if you're both
in the same one). 

## Management

First of all, if you think you can write your own time management
software library, or even write a little routine that adds or subtracts
arbitrary values from the time of the day,
please allow me to show you the source code for the
[java.util.Date](http://www.docjar.com/html/api/java/util/Date.java.html)
and
[java.util.GregorianCalendar](http://www.docjar.com/html/api/java/util/GregorianCalendar.java.html)
classes from JDK 7, respectively weighting 1331 and 3179 lines of code. 

Okay, these are probably not the best examples of software routines that
deal with time, I agree. That's why Java libraries like 
[Joda Time](http://joda-time.sourceforge.net/) were written (so you don't have
to)! In fact, Joda Time has become so popular that it gave birth to
[JSR-310](http://jcp.org/en/jsr/detail?id=310) and is
[now](http://www.h-online.com/open/news/item/JSR-310-s-Date-and-Time-API-added-to-JDK-8-1708647.html)
[part](http://www.infoq.com/news/2013/02/java-time-api-jdk-8) of JDK 8.

Use of popular, well designed and implemented time frameworks will save
your life. Seriously. Take your time to get familiar with the API of
your choosing. If you are into Scala, I can recommend
[nscala-time](https://github.com/nscala-time/nscala-time). And if you
are into other programming languages, honestly, I have no idea, but ask
your local guru: she can help.

## Further Resources

Here are a few useful links I've accumulated over time:

* [Falsehoods programmers believe about time](http://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)
* [The 5 laws of API dates and times](http://apiux.com/2013/03/20/5-laws-api-dates-and-times/)
* [Storing Date/Times in Databases](http://derickrethans.nl/storing-date-time-in-database.html)
* [A Short History of the Modern Calendar](http://youtu.be/kzprsR2SvrQ)
* [Converting world timezones with DuckDuckGo and Wolfram Alpha from the browser address bar](http://opensourcehacker.com/2013/03/28/converting-world-timezones-with-duckduckgo-and-wolfram-alpha/)
