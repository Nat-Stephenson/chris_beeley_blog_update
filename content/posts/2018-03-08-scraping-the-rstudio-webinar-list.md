---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-03-08T18:47:49Z"
title: Scraping the RStudio webinar list
---

I only just found this list of [RStudio webinars](https://www.rstudio.com/resources/webinars/), there’s loads of stuff on there, I really need to plow through a lot of it. What I really wanted was a list of them with links that I could archive and edit and rearrange so I could show which ones I am interested in, which I’ve already watched, and so on.

Well, if you’ve got a problem, and no-one else can help, then maybe you need… The R Team.

<div class="jetpack-video-wrapper"><span class="embed-youtube" style="text-align:center; display: block;"><iframe allowfullscreen="true" class="youtube-player" height="372" loading="lazy" sandbox="allow-scripts allow-same-origin allow-popups allow-presentation" src="https://www.youtube.com/embed/7d_dWs7fdk4?version=3&rel=1&showsearch=0&showinfo=1&iv_load_policy=1&fs=1&hl=en-US&autohide=2&wmode=transparent" style="border:0;" width="660"></iframe></span></div>Anyway, that’s enough nostalgia. So all we need is the mighty [rvest package](https://cran.r-project.org/web/packages/rvest/index.html) and just a little sprinkling of paste0() and we’re away.

Oh yes, and you’ll also need selector gadget, which is described brilliantly in this [selector gadget vignette](https://cran.r-project.org/web/packages/rvest/vignettes/selectorgadget.html).

Once you’ve got all that, the code writes itself. The only wrinkle I ironed out was that some of the HTML paths were relative, not absolute, so I paste http://blah on the front of those ones, as you’ll see.

```
<pre class="brush: r; title: ; notranslate" title="">

library(rvest)

rstudio = read_html("https://www.rstudio.com/resources/webinars/")

linkText = rstudio %>%
  html_nodes('.toggle-content a') %>%
  html_text()

linkURL = rstudio %>%
  html_nodes(".toggle-content a") %>%
  html_attr("href")

linkURL[substr(linkURL, 1, 4) != "http"] = 
  paste0("https://www.rstudio.com", 
         linkURL[substr(linkURL, 1, 4) != "http"])

cat(paste0("<a href = ", linkURL, ">", 
           linkText, "</a><br>"), file = "webinar.html")

```

Done! Now all I did was open the resulting file and paste it into Evernote, which kept the links and text together, as you’d expect, and I can now cut and paste and markup to my heart’s desire.

I love it when a plan comes together.