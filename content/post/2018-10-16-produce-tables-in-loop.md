---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-10-16T16:23:37Z"
guid: https://chrisbeeley.net/?p=1159
id: 1159
title: Produce tables in loop
url: /?p=1159
---

Another one for me before I forget, I seem to have terrible problems writing headings and tables in a loop in RMarkdown. Everything gets crushed together because the line returns don’t seem to work properly and either the table or the headings get mangled. Well here it is, the thing I just did that worked:

```
<pre class="brush: r; title: ; notranslate" title="">

for(i in 1:2){

  cat("Heading ", i, "\n")
  
  cat("===========================")
  
  pandoc.table(iris)
}

```

That’s it! Don’t put any other weird “\\n”s or \[HTML line breaks which I now realise WordPress will interpret literally\] in. You don’t need them!