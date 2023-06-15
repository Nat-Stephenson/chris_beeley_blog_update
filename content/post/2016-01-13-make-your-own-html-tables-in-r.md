---
author: chrisbeeley
categories:
- Uncategorized
date: "2016-01-13T14:53:54Z"
guid: http://chrisbeeley.net/?p=796
id: 796
tags:
- HTML
- R
- tables
title: Make your own HTML tables in R
url: /?p=796
---

I’ve avoid writing HTML tables by hand in R, reasoning that it would be far too complicated. But the problem with using packages like pander and rmarkdown, much as I love those packages, is you can’t fine tune the outputs. So if a colleague asks you to add little extra touches for a report you can’t really do it.

So it was a nice surprise to find that it’s actually not that hard. Here’s a table with 3 columns which looks very nice with bootstrap (don’t know what the styling options are on WordPress so it just looks unformatted here)

| Service quality | 95 | 90 |
|---|---|---|
| Promoter | 90 | 85 |
| SUCE returns | 1000 | 1200 |

Code:

```
<pre class="brush: r; title: ; notranslate" title="">

paste0(c("<table class='table table-striped'>", 
  "<th>Score</th><th>Current quarter</th><th>Previous quarter</th>",
  "<tr>", paste0("<td>", c("Service quality", 95, 90), "</td>"), "</tr>",
  "<tr>", paste0("<td>", c("Promoter", 90, 85), "</td>"), "</tr>",
  "<tr>", paste0("<td>", c("SUCE returns", 1000, 1200), "</td>"), "</tr>",
  "</table>"), collapse = "")

```

Obviously you can make it dynamic pretty easily, the table above is from a Shiny app I’m currently developing, I’ve just replaced the numbers with static values for the blog post. So the next time someone asks me to style a table it should be pretty easy.