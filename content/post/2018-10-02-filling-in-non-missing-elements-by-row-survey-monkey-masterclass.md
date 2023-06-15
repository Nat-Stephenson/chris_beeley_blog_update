---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-10-02T10:17:27Z"
guid: https://chrisbeeley.net/?p=1155
id: 1155
title: Filling in non missing elements by row (Survey Monkey masterclass)
url: /?p=1155
---

There must be other people out there doing this, so I’ll share this neat thing I found today. Quite often, you’ll have a dataset that has a lot of columns, only one of which will have something in it for each row. Survey Monkey, in particular, produces these kinds of sheets if you use branching logic. So if you work in one bit of the Trust, your team name will be found in one column, but if you branched elsewhere in the survey because you work somewhere else, it is in another column. Lots of columns, but each individual has only one non-missing element in each (because they only work in one place and only see that question once).

In the past I’ve done hacky things with finding non missing elements in each column line by line and then gluing it all together. Well today I’ve got 24 columns so I wasn’t going to do it like that today. Sure enough, dplyr has a nice way of doing it, using [coalesce](https://rdrr.io/cran/dplyr/man/coalesce.html).

```
<pre class="brush: r; title: ; notranslate" title="">

coalesce(!!!(staffData %>%
               select(TeamC1 : TeamC24)))

```

That’s it! It accepts vectors of the same length or, as in this case, a dataframe with !!! around it. Bang bang bang (as it’s pronounced) is like bang bang (!!) except it substitutes lots of things in one place instead of one thing in one place. So it’s like feeding coalesce a lot of vectors, which is what it wants.

I really can’t believe it’s that simple.