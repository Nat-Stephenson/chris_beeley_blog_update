---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-02-03T17:45:39Z"
guid: https://chrisbeeley.net/?p=1188
id: 1188
title: Dplyr function to replace nested ifelse (like SQL CASE WHEN)
url: /?p=1188
---

I hate nested ifelse statements in R code. I absolutely hate them. They are just ugly and difficult to read. I found this lovely function in dplyr called case\_when (which is like the SQL CASE WHEN if you know that) that banished them forever. It’s easier if I just show you (this is from the documentation)

```
<pre class="brush: r; title: ; notranslate" title="">

x <- 1:50
case_when(
  x %% 35 == 0 ~ "fizz buzz",
  x %% 5 == 0 ~ "fizz",
  x %% 7 == 0 ~ "buzz",
  TRUE ~ as.character(x)
)

```

How wonderful is that? No more nested ifelse!