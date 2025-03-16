---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-01-12T15:12:35Z"
guid: http://chrisbeeley.net/?p=871
id: 871
title: Concatenating text nicely with commas and &#8220;and&#8221;s
url: /?p=871
---

Wow, this blog is quiet at the moment. I can’t even remember if I’ve written this anywhere but I had a liver transplant in May and I’ve been repeatedly hospitalised with complications ever since. So work and life are a little slow. I’m hoping to be back to full health in a few weeks.

Anyway, I was just writing some Shiny code and I have a dynamic text output consisting of one, two, or many pieces of text which needs to have commas and “and”s added to make natural English, like so:

Orange. Orange and apple. Orange, apple, and banana. Orange, apple, banana, and grapefruit.

I really toyed with the idea of just using commas and not worrying about “and”s but I decided that it was only a quick job and it would make the world more attractive. So if you’ve just Googled this, here’s my solution:

```
<pre class="brush: r; title: ; notranslate" title="">

theWords = list(LETTERS[1], LETTERS[2:3], LETTERS[4:6])

vapply(theWords, function(x) {

if(length(x) == 1){

x = x # Do nothing!

} else if(length(x) == 2){

x = paste0(x[1], " and ", x[2])
} else if(length(x) > 2){

x = paste0(paste0(x[-length(x)], ", ", collapse = ""),
"and ", x[length(x)])
}

}, "A")

```

Notice the use of vapply, not lapply, because Thou Shalt Use Vapply.