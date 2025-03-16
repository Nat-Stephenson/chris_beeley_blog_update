---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-04-17T12:39:42Z"
guid: https://chrisbeeley.net/?p=1092
id: 1092
title: Convert icons to different colours
url: /?p=1092
---

I’m horribly, horribly busy (stay tuned until mid May-ish to hear about what I’ve been up to) but I’ve just done something so quickly and easily that I couldn’t resist sharing it.

So I’ve downloaded a little man icon, I’m going to use it in a pictogram to show the percentage of people who agree with something. Just a bit of fun for a presentation. The man is black, but I thought it would be nice to have green men who are saying happy things (“I felt supported”, that kind of thing) and red men who are saying unhappy things (“I felt isolated”, e.g.).

I was a bit worried it would eat up too much time. I read [this article about Imagemagick](http://www.imagemagick.org/Usage/color_basics/#replace). I typed this into my Linux terminal:

```
<pre class="brush: plain; title: ; notranslate" title="">

convert blackicon.png -fuzz 40% -fill green -opaque black greenicon.png

```

Boom. Finished. God bless you, Linux.