---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-05-23T14:17:18Z"
title: Lazy tables with R and pander
---

One of the many things I love about R is how gloriously lazy it can help you to be. I’m writing a report at the moment and I need to make lots of tables in R Markdown. I need them to be proportions, expressed as a percentage, rounded to 0 decimal places, and I need to add (%) to each label on the table. That’s a lot of code when you’ve got 8 or 10 tables to draw, so I just made a function that does it. It takes two arguments, the variable you want tabulated, and the order in which you want the table. I need to specify the order manually because the default alphabetical ordering doesn’t work with all of the data that I want, as in the example here. Without ordering manually, the “Less than one year” category appears at the end.

Here’s a minimal example:

```
<pre class="brush: r; title: ; notranslate" title="">

library(pander)

niceTable = function(x, y) {
  
  tempTable = round(
    prop.table(
      table(
        factor(x, 
               levels = y)
      )
    ) * 100, 0)
  
  names(tempTable) = paste0(names(tempTable), " (%)")
  
  pandoc.table(tempTable)
  
}

a = c(rep("Less than one year", 3), rep("1 - 5 years", 4), 
    rep("5 - 10 years", 2))

niceTable(a, c("Less than one year", 
    "1 - 5 years", "5 - 10 years"))

```

Boom! Instant laziness. Unless my boss is reading, in which case it’s efficiency 🙂