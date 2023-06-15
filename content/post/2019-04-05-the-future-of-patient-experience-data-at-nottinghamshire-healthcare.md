---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-04-05T10:50:20Z"
guid: https://chrisbeeley.net/?p=1214
id: 1214
title: The future of patient experience data at Nottinghamshire Healthcare
url: /?p=1214
---

I’ve talked about my plans for patient experience data in quite a few different forums recently, but it just occurred to me that it isn’t written down anywhere.

Anyone who’s spent any time with me in meetings will know I’m a stickler for writing things down, so here it is.

I launched a Shiny application to explore our patient experience data about 6 years ago. It was pretty early days for Shiny and it seemed pretty whizzy and exciting at the time. Over time I added more features but it seemed less and less whizzy and exciting, and to be honest I was too busy nearly dying twice to worry about it (see this blog, passim, don’t worry I’m absolutely fine now)·

Anyway, I’ve now launched a new version built with [shinydashboard](https://rstudio.github.io/shinydashboard/) which can be found here <http://suce.co.uk:8080/apps/SUCE/>. It launched a bit early at an event, so excuse any rough edges or bugs.

So what’s the big idea, then? Well, it’s twofold. Firstly, I would like to integrate the staff experience data. My Trust has recently started staff about the experience they have working at the Trust and I have made sure that these datasets are tightly integrated. Ultimately I would like to build a dashboard that shows how staff and patients are on the same application. This will be of interest, e.g. to ward managers, who want to know how their staff and patients are. Further, it will facilitate analysis of the connection between staff and patient experience. The key design feature is summarising the data and allowing rapid drill down. So a service manager would start on a page with staff and patient experience data, but would notice something interesting, click it, and then be presented with more detail about that thing. Further clicks would lead to further drill down.

So far, so ordinary. This is what all those private sector dashboard salespeople with the expensive watches they bought with taxpayers’ money all say. What I want to add as secret sauce (which will of course be open source, haha) is what I’ve taken to calling a recommendation engine for patient feedback. So when you buy something on Amazon it will say “people who bought that bought this, too.” And then you buy it and love it (I’ve bought loads of stuff using the Amazon recommendation engine. And listened to great music using Spotify’s).

The big problem we have with experience data is discoverability. We have maybe 200,000 comments. How do you find the one you want? I’m trying to use machine learning to have the computer find other things you might be interested in. Reading some feedback about waiting lists? Here’s some more. Reading angry feedback? Here’s some more angry feedback. And feeding this engine staff and patient feedback data could make for a very powerful system. Lots of people are angry at their doctor? Well, look, all the doctors are fed up to. Join it with other systems. Food’s terrible? Well, look, 10% of the estates staff are off sick at the moment. 10% of the estates staff are off sick? Well, look, three months ago they were all miserable. And so on.

It’s a very ambitious dream, but that’s my vision, and I’ll try as far as possible to build technologies that other Trusts can use. That is in itself quite tricky, so progress with that one in particular might be slow, but I’m trying to get more people on board. See how it goes.