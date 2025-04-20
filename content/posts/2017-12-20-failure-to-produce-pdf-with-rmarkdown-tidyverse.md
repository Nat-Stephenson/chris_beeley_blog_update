---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-12-20T15:21:32Z"
title: Failure to produce pdf with RMarkdown tidyverse
---

I’m using tidyverse for everything now, as I’ve mentioned in previous posts, when I want a cup of tea I just run:

```
<pre class="brush: r; title: ; notranslate" title="">

house %>%
  filter(kitchen == 1) %>%
  select(tea, kettle) %>%
  infuse()

```

I just ran [code](https://gist.github.com/ChrisBeeley/867ffe93f541797e48f3ad195a75efb9)(in a gist because the markup makes Hugo go strange) in a vanilla RStudio setup with pdflatex installed

This is the code that you get if you set up an RMarkdown project in RStudio and select “compile to LaTeX”, and you want to do some data stuff with the tidyverse package.

And it produced the following error message:

! Package inputenc Error: Unicode char √ (U+221A)  
(inputenc) not set up for use with LaTeX.

See the inputenc package documentation for explanation.  
Type H <return> for immediate help.  
 … </return>

l.145 \\end{verbatim}

Try running pandoc with –latex-engine=xelatex.  
pandoc: Error producing PDF  
Error: pandoc document conversion failed with error 43  
Execution halted

I was a bit confused by this for quite a while, the answer of course turns out to be the lovely messages which the tidyverse produces on loading:

![](https://lh3.googleusercontent.com/-Cn0ZuIwP3zs/Wjp1shrKdeI/AAAAAAAAL7Y/rcFYBh2GvA8sw8jgcUcwheZ0aertVHX4gCHMYCw/d/Screenshot_.jpg)

With the default message = TRUE behaviour in the code chunk pandoc ends up trying to render those little ticks in LaTeX. Evidently it doesn’t support unicode.

So the document fails, and it’s hard to understand why until you knit to HTML and see the little ticks.

Changing the knitr::opts\_chunk$set(echo = TRUE) line to knitr::opts\_chunk$set(echo = TRUE, message = FALSE) fixes the problem.

I can’t help but think that this is a rare example of R getting harder to use. When I started with R 10 years ago it was much more difficult to do even simple things like load a csv file or work with dates. These days there are lots of lovely packages to help, and of course RStudio itself makes using R much more intuitive. But this is going to confuse newbies, I think, which is a bit of a shame.

There are several obvious fixes, I won’t bother to list them all, maybe make message = FALSE the default in RMarkdown documents in RStudio seems like the best one, but maybe there’s some reason they don’t want to do that.