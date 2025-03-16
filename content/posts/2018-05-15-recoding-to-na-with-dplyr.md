---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-05-15T10:54:04Z"
guid: https://chrisbeeley.net/?p=1098
id: 1098
title: Recoding to NA with dplyr
url: /?p=1098
---

Just very quickly again, still horribly busy, but this has been annoying me for ages and I finally figured it out. When you’re using recode from dplyr, you can’t recode to NA if you’re recoding to other things that aren’t NA, because it complains that the types aren’t compatible. So don’t do this:

```
<pre class="brush: r; title: ; notranslate" title="">

  mutate(Relationship = recode(Relationship, "Co-Habiting" = "Co", 
    "Divorced" = "Di", "Married" = "Ma", "Civil partnership" = "CI"
    "Single" = "Si", "Widowed" = "Wi", "Separated" = "Se", 
    "Prefer not to say" = NA))

```

Use na\_if() instead:

```
<pre class="brush: r; title: ; notranslate" title="">

  mutate(Relationship = recode(Relationship, "Co-Habiting" = "Co", 
    "Divorced" = "Di", "Married" = "Ma", "Single" = "Si", 
    "Widowed" = "Wi", "Separated" = "Se", "Civil partnership" = "CI")) %>%
  mutate(Relationship = na_if(Relationship, "Prefer not to say", NA))

```