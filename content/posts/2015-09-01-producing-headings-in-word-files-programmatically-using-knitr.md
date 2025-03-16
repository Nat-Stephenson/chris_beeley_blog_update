---
author: chrisbeeley
date: "2015-09-01T15:54:49Z"
title: Producing headings in Word files programmatically using knitr
---

I love [knitr](http://yihui.name/knitr/). Sometimes I have to send people documents in Word. It works beautifully except when I want to produce headings programmatically in code using cat(). This is fine in HTML:

{{< highlight R  >}}

```
sapply(headings, function(x){
    
  cat(paste0("<h1>", x, "</h1>"))
    
  cat(paste0("<p>", rep("Some text here", 10), "</p>"))
    
})

```
{{< / highlight >}}

However, when writing Word .docx files using [Rmarkdown](http://rmarkdown.rstudio.com/) the heading tags are not recognised. Well, they’re not printed, so they obviously are recognised, but they don’t work.

I’ve struggled for a long time to understand how to produce headings in Rmarkdown and to be honest in the past I’ve actually done it with a lot of guesswork and some silly hacky unnecessary code. Today I have finally figured out a foolproof way to do it very easily every time, so I present the [whole document](https://gist.github.com/ChrisBeeley/90889f6d8c4746af2fbf017117e6603c)(as a gist because the markup makes Hugo go weird) to you and to my future self when I inevitably forget how to do it.

That’s it! Don’t forget the invisible() because otherwise if you use lists and things like that you’ll get the names of the list at the end.

