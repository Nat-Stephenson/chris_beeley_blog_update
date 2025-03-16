---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-12-23T21:46:03Z"
guid: http://chrisbeeley.net/?p=710
id: 710
title: I did it! I edited some open sourced code!
url: /?p=710
---

I do try to promote the benefits of open source wherever I go, and one of these many benefits is, of course, the ability of endusers or intermediate parties to modify the source code to make changes to the software. A couple of times I have been dismissed by members of the closed source contingent who mock the idea that individuals who work outside of the IT world (e.g. in healthcare, my own area) would sit down and modify or even read reams of source code.

I think this is a rather silly argument, but that needn’t detain us here. I am writing today merely to say that I have now finally, with my own two hands, modified some source code because the original program didn’t do what I want.

Admittedly, the program in question is part of an R package and the function I modified is really quite small and written in R (my strongest language by far) but still, the point stands. A statistic I wanted was being output in a very silly and annoying way. If I were using STATA or SPSS I could probably have fixed the problem but I would have had to capture the output as text and then munge it by hand, which would have taken absolutely ages. Instead I just fired up the source, fiddled with a few lines, reran the function, and I was finished.

It’s proof of concept for me, at least. May it be the first of many changes, and one day hopefully I can actually make myself useful and make a [pull request](http://yangsu.github.io/pull-request-tutorial/).