---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-03-29T13:27:27Z"
guid: http://chrisbeeley.net/?p=609
id: 609
title: Produce Word documents, web pages, and pdfs in R quickly and easily
url: /?p=609
---

When I started using R in about 2009 [reproducible research](http://users.stat.umn.edu/~geyer/Sweave/) was a complete eye-opener for me. Statistical analyses should be reproducible both by the author of them and by the scientific community and by embedding the analysis and outputting code within a [literate programming](http://www-cs-faculty.stanford.edu/~uno/lp.html) framework such as [Sweave](http://en.wikipedia.org/wiki/Sweave) this was possible. Nearly everything I produce now, whether for regular reports on patient experience for the NHS Trust for which I work or for scientific papers is reproducible. These tools have saved me (and therefore my employer) hundreds of hours and they are all totally free.

Having said all that, there has recently been a sea-change in the R world as far as reproducible research goes. Although I could produce beautiful pdfs in LaTeX I often needed to write Word documents to share with colleagues. I would work in LaTeX/ Sweave through all the early stages and then use [LaTeX2RTF](http://latex2rtf.sourceforge.net/) to produce an RTF which I would then save as a .doc. It was okay, a bit fiddly, and the reproduction was not always perfect, but even then these free tools were saving me hundreds of hours.

The advent of [knitr](http://yihui.name/knitr/), [RMarkdown](https://www.rstudio.com/ide/docs/r_markdown), and [RStudio](http://www.rstudio.com/) has made the whole process embarassingly easy. Load RStudio, select File… New… RMarkdown, produce your report based on the template and the help files which are embedded right in RStudio for you to use, click “Knit to HTML” and you’re done. Want to share your results? Click “Publish” and put them straight onto the internet, send a link to your collaborators.

But it gets better. I’ve been hearing about [pandoc](http://johnmacfarlane.net/pandoc/) for some time but have always been worried that it would prove complicated and wouldn’t immediately repay the investment. How wrong I was.

Pandoc turns just about any document into just about any other type of document, I haven’t even scratched the surface, but I’ve been using it to produce nicely formatted word documents. Install pandoc (sudo apt-get install pandoc on Ubuntu, GIYF for other OS’s), write an RMarkdown document just as you did before, and run the following:

```
<pre class="brush: r; title: ; notranslate" title="">

library(knitr)

knit("myDoc.Rmd", "myDoc.md")

system("pandoc -o myDoc.doc myDoc.md")

```

Boom. Done. I get a weird thing where the figures are referenced to file locations on my computer, so when I email it to people the figures disappear. I’m sure there’s a clever way to deal with this, I just open the document and “Save As” a Word document elsewhere and the figures save in the file in the new location.

I cannot over-emphasise how quick and easy this is. I would advise anybody reading this who has even the slightest interest in any of this to jump immediately on the RMarkdown train. You can even publish [straight to WordPress](http://yihui.name/knitr/demo/wordpress/).