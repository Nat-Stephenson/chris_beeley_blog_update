---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-11-13T17:07:50Z"
guid: https://chrisbeeley.net/?p=1170
id: 1170
title: Drawing stacked barcharts the tidyverse way
url: /?p=1170
---

Don’t judge me, but I do spend quite a lot of time making stacked barcharts of survey data. I’m trying to go full tidyverse (see this blog, passim) and there is a very neat way of doing it in the tidyverse which is very easy to expand out into functions, or purrr, or whatever. Of course, me being me I can never remember what it is so I end up reading the same SO answers over and over again.

So here, for all time, and mainly for my benefit, is the code to take a dataframe with the same Likert items over and over (in this case, Always/ Usually/ Sometimes/ Never) and find the proportions, change to percentages, make the x axis labels at an angle so they fit, put the stacking in the right order, and remove missing values. It’s pretty much completely generic, just give it a dataframe and the factor levels and it will work with any dataset of lots of repeated Likert items.

```
<pre class="brush: r; title: ; notranslate" title="">

theData %>% 
  gather() %>% 
  group_by(key) %>% 
  count(value) %>%
  filter(!is.na(value)) %>% 
  mutate(prop = prop.table(n) * 100) %>%
  mutate(value = factor(value, levels = c("Always", "Usually", "Sometimes", "Never"), 
                        labels = c("Always", "Usually", "Sometimes", "Never"))) %>% 
  ggplot(aes(x = key, y = prop, fill = value, order = -as.numeric(prop))) + 
  geom_bar(stat = "identity") + 
  theme(axis.text.x = element_text(angle = 45, vjust = 1, hjust = 1))
```

Et voila!

![Rplot01](https://chrisbeeley.net/wp-content/uploads/2018/11/Rplot01.png)