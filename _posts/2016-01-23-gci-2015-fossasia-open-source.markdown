---
layout: post
title: Google Code-in 2015, Fossasia, Open source
excerpt: "My experience in participating in Google Code-in 2015 and joining the world of open source"
categories: [Blog, Open source]
modified: 2016-01-23
comments: true
---

GCI 2015
--------

It's 23 of January today, 2 more days before the end of GCI2015. In this article I would like to write about the experience I had, ideas and some general emotions :)


It all started in November 2015. I heard about GCI from a post at [habrahabr.ru](https://habrahabr.ru) and understood that it's a good way to code something not just for me but also to be involved in something big and interesting.  I definitely wasn't mistaken. I googled for GCI and understood that the contest was going to start in a month. After a month, on December 7th I registered for the  competition and the journey began.

![GCI landing](http://s19.postimg.org/6vm75rceb/GCI_landing.png)

The grass is always greener on the other side of the hill
---------------------------------------------------------

Before starting to do something I spent a few minutes to read GCI FAQ page, usually I don't read FAQ pages but this time I thought that it's something must-read. Probably the most useful idea one can get from FAQ is that in order to win one have to choose only 1 organization and work for it as this increases chances to win. That's usually where everything interesting starts. 

> Which organization is the best for me?

> Which one do I have to choose?

I think all of the participant had these questions. Thinking for a bit leads to understanding that it's necessary to choose an organization not only you *want* to contribute to, but also you *can* contribute to. Usually organizations' landing pages provide info about technology stack they use and it's definitely the best place to get started.

![GCI organizations](http://s19.postimg.org/w470zfhc3/GCI_organizations.png)

After analyzing what I am good at and checking different organizations I understood that speaking about technology stack I could contribute to several organizations. I stopped on [FOSSASIA](http://fossasia.org/) as there I found the most interesting tasks for me.

FOSSASIA
--------

![FOSSASIA](https://upload.wikimedia.org/wikipedia/commons/thumb/6/62/FOSSASIA_Logo.svg/2000px-FOSSASIA_Logo.svg.png)

First of all, I would like thank all the FOSSASIA mentors who were involved in the project. It really has to be appreciated, these are the people who helped others not for some profit, but just because of the idea.

It was really interesting to speak with the mentors and other GCI participants in the IRC, the place where one could help others, the place where things were created.

![FOSSASIA IRC](http://s19.postimg.org/gt1tck9dv/GCI_IRC.png)

Code is worth a thousand words
------------------------------

Yet another very interesting part GCI is of course coding. FOSSASIA team designed about 282 tasks for students from around the world, there was something interesting for everyone.

Probably my favorite task was "Script: Scrape Public Parliament Data using Python (FOSSASIA)". I'd done much scrapping before and honestly it's not the hardest task to do, but the thing I understood from the task is that  volumes of parsing can differ. It's easy to parse 1, 2 or 20 pages but when it comes to 10400 pages and ~104000 `*.pdf` and `*.doc` files everything starts to take too much time. I wrote a basic parser using python `requests` and `beautifulsoup4` in about 30 minutes, but it took really much time to parse even small amounts of data. I understood that it has to work faster, much faster, and spent about 2 days learning about threading and asynchronous http requests. In the end I was able to parse  about 3-6 pages/sec. I was really happy because of this as I could scrape 0.1 million files from the site in an hour. Besides, [here](https://github.com/fossasia/parliament-scaper/tree/master/python-async) is the source code for the implementation.

Conclusion
----------

In conclusion I would like to say that Google Code-in is definitely must-have experience for young developers. It's the place where you can start your journey to the real world of programming beginning with some open source. GCI doesn't teach how to code, it teaches to collaborate and the contest is not that much about competition as it is about being a part of the community. It definitely gave me more love to programming, ideas for my own open source projects, showed me how it's possible to work in a team and made my [GitHub profile](https://github.com/pythad) a bit greener :D

![pythad GitHub](http://s19.postimg.org/k54pjlqir/GCI_Git_Hub_profile.png)

One more time thanks to the people who made this contest exist. Now it's the time to wait for results and wish each other good luck!
