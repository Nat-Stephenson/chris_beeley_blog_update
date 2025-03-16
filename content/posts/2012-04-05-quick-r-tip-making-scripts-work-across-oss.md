---
author: chrisbeeley
categories:
- Uncategorized
date: "2012-04-05T14:11:35Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=205
id: 205
publicize_results:
- a:1:{s:7:"twitter";a:1:{i:346491435;a:2:{s:7:"user_id";s:11:"ChrisBeeley";s:7:"post_id";s:18:"187905346998779904";}}}
title: Quick R tip- making scripts work across OS&#8217;s
url: /?p=205
---

Quick thought for newbies to R. I have spent four years loading in data like this

```
<pre class="brush: r; title: ; notranslate" title="">
mydatafirst=read.csv("D:\Dropbox\R-files\Project1\etc.")
```

 on my Windows machine and like this

```
<pre class="brush: r; title: ; notranslate" title="">
mydatafirst=read.csv("/home/chris/Dropbox/R-files/Project1/etc.")
```

on my Linux machine. I would comment out the one I wasn’t using at the time.

It’s fine for a while but once you start reading multiple dataframes (particularly if they’re not all loaded at the start) it gets very fiddly and annoying. Don’t do this. Just set your working directory. You can do it manually:

setwd(“D:\\Dropbox\\R-files\\etc.”)

Or, even better, the seamless option:

```
<pre class="brush: r; title: ; notranslate" title="">
setwd(ifelse(Sys.info()['sysname']=="Windows";,
             "D:\Dropbox\R-files\Project1\",
             "~/Dropbox/R-files/Project1/"))
```

Mmm, I can almost taste the hours I’ll save in the course of a year.