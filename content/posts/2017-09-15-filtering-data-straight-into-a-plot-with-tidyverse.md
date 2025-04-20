---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-09-15T09:05:04Z"
title: Filtering data straight into a plot with tidyverse
---

I’m still trying to go full tidyverse, as I believe I mentioned a while back. It’s clearly a highly useful approach, but on top of this I see a load of code in blogs and tutorials that uses a tidy approach. So unless I learn it I’m not going to have a lot of luck reading it. I saw somebody do the following a little while back and I really like it so I thought I’d share it.

In days gone by I would draw lots of graphs in an RMarkdown document like this:

```
<pre class="brush: r; title: ; notranslate" title="">

firstFilteredDataset = subset(wholeData, 
  Date > as.Date("2017-04-01"))

ggplot(firstFilteredDataset, 
  aes(x = X1, y = y)) + geom_... etc.

secondFilteredDataset = subset(wholeData, 
  Date > as.Date("2015-01-01"))

ggplot(secondFilteredDataset, 
  aes(x = X1, y = y)) + geom_... etc.

thirdFilteredDataset = ... etc.

```

It’s fine, there’s nothing wrong with doing that, really. The two drawbacks are firstly that the code looks a bit ungainly, creating lots of objects that are used once and then forgotten about, and secondly it is filling your RAM with data. Not really a problem on my main box, which has 16GB of RAM, but it’s a bad habit and you may come unstuck somewhere else where RAM is more limited- like for example when you’re running code on a server.

So I saw some code on the internet the other day and they just piped data straight from a dplyr filter statement to a ggplot instruction. No muss, no fuss, the data is defined in the same function in which it’s used, and you’re not making loads of objects and leaving them lying around. So here’s an example:

```
<pre class="brush: r; title: ; notranslate" title="">

library(tidyverse)

mpg %>% 
  filter(cyl == 4) %>%
  group_by(drv) %>%
  summarise(highwayMilesPG = mean(hwy)) %>%
  ggplot(aes(x = drv, y = highwayMilesPG)) +
  geom_bar(stat = "identity")

```

There’s only one word for it- it’s tidy! I like it!