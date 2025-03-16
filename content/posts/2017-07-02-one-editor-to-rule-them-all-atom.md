---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-07-02T09:57:15Z"
guid: http://chrisbeeley.net/?p=1023
id: 1023
title: One editor to rule them all- Atom
url: /?p=1023
---

I’m very happy using RStudio for all my R code. It goes without saying that the support for R coding built into RStudio is phenomenal. If you don’t know loads of cool stuff RStudio does, you’re missing out, but that’s a blog post on its own.

I’ve never quite been happy with my choice for other general editing, though. Sometimes I write PHP, HTML, markdown, Python, or something else, and I’ve never really found an editor that I love. [Geany](https://www.geany.org/) is pretty good and that’s what I have been using when I write PHP or HTML. I tended to write markdown in RStudio, which is kind of stupid, since RStudio is an awfully big hammer to crack that nut, but it does support markdown and I’m familiar with RStudio, so I was happy enough doing that. I never really found a Python IDE that I loved. As far as I can tell there isn’t really an RStudio equivalent in the Python world, something so well featured and brilliant that it’s really the only choice unless you have a very particular reason to use something else.

So about a year ago I gave [Atom](https://atom.io/) a try. It had been out of beta for about a year by that point. I don’t really remember it clearly now but it seemed a bit clunky and I just rapidly gave up (to be fair, this may have just been me being thick, I’ve no idea how much it has really improved since). It just didn’t grab me. I keep seeing it mentioned everywhere and I thought I would give it another go.

This time I was hooked straight away. It’s described as a “hackable editor for the 21st Century” and that’s the real strength of it. The actual interface is very clean and simple, no bells and whistles, but it comes bundled with some plugins and there is a thriving ecosystem of user contributed packages that can make Atom, it seems so far, anything you want it to be.

I think I love Atom for the same reason I love R. It has a big ecosystem of packages around it, and whatever problem you want to solve, as Apple almost said of the iPhone, “there’s a package for that”.

Your needs will be different from mine, of course, but I recommend you give Atom a try if you haven’t already. It supports Markdown preview out of the box. So far I have installed two packages- platformio-ide-terminal, and script. Platformio-ide-terminal allows you to spawn a terminal underneath your code window, which I have been mainly using to run pandoc on my markdown files. Script will run your code for you (sections, the whole thing, etc.) all with a shortcut key. So far I’ve been using that for testing Python scripts. Oh yes, and the markdown editor supports word completion out of the box too, not code, just normal words, which is more useful than it sounds.

While I’ve been Googling Atom to find the links to put in this post I have found two really cool things that I didn’t know about. Firstly, there is the Hydrogen package, which allows Jupyter like functionality in Atom. If you don’t know what Jupyter is, you should find out, but essentially it allows you to weave together your code and output, just like you can with RMarkdown.

And secondly Atom themselves have just released [teletype](http://blog.atom.io/2017/11/15/code-together-in-real-time-with-teletype-for-atom.html) which is a tool that allows collaboration on code files right inside the Atom editor. I don’t really need to do that, not that I can think of anyway, but you have to admit it’s pretty awesome. They’ve solved a lot of the problems with code collaboration elsewhere, as well, have a look at the blog post for more details.

So go give Atom a try. I’ll try to post any more Atom-related awesomeness that I see on my travels.