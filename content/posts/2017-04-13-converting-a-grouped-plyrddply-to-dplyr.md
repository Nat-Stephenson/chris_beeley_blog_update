---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-04-13T16:19:59Z"
title: Converting a grouped plyr::ddply() to dplyr
---

I’m going full [tidyverse](http://tidyverse.org/) at the moment and so I’m converting my old plyr code to dplyr. It’s been pretty steady going so far, although I had a bit of difficulty converting an instruction using ddply which carried out a function based on a subgrouping within the data. I wrote a toy example to get it right, I may as well share it with the internet in case it helps someone else. If you can’t figure out what I’m talking about with what the function does, just run the code. You’ll see what it is. Easier than explaining in words. The following two functions carry out the same task, the latter is a translation of the former.

```
library(plyr)
library(tidyverse)

plyr::ddply(mtcars, "cyl", mutate, mean.mpg = mean(mpg))

mtcars %>% 
  dplyr::group_by(cyl) %>% 
  dplyr::mutate(mean.mpg = mean(mpg))

```