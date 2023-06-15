---
author: chrisbeeley
categories:
- Uncategorized
date: "2021-01-29T22:47:49Z"
guid: http://chrisbeeley.net/?p=1478
id: 1478
title: Accessing plot characteristics in Shiny modules
url: /?p=1478
---

It was my New Year’s resolution to blog more and that’s going really well because this is the first blog I’ve done all year and it’s nearly the end of January. Well, I suppose to be fair I did do [a post on our team blog](https://cdu-data-science-team.github.io/team-blog/posts/2021-01-20-a-new-github-release-and-future-projects/) but that feels like I’m making excuses.

Anyway, never mind! This is a quick blog post because this problem I had the other day has, as far as I can tell, absolutely no Google juice at all and it stumped me for ages, even though it is very simple.

We wanted to [access the height of a Shiny plot at runtime](https://github.com/rstudio/shiny/issues/650) but we couldn’t get it to work in a Shiny module. I googled all sorts of stuff, “session$clientData module”, “accessing plot characteristics shiny module”, “session variable shiny module”, all kinds of things, but it just didn’t work. I figured the namespacing was probably messing it up but I couldn’t figure it out. The simple version would have been session$clientData$output\_sentiment\_plot\_upset\_width, but I wasn’t surprised that that didn’t work because of the namespacing.

When I looked at the raw HTML (which I did, in desperation) I saw that the input was actually called “output\_mod\_sentiment\_ui\_1-sentiment\_plot\_upset”. The mod\_sentiment bit is the name spacing. So I got pretty close when I tried session$clientData$output\_mod\_sentiment\_ui\_1-sentiment\_plot\_upset\_width. But R thought that the hyphen was a minus, so that didn’t work either. Then the light dawned

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: r; title: ; notranslate" title="">
session$clientData$`output_mod_sentiment_ui_1-sentiment_plot_upset_width`
```

</div>That’s it. Simple, but it took me a while, so hopefully if you have the same problem you will find this post.