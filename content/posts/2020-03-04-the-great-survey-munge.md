---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-03-04T13:51:00Z"
title: The Great Survey Munge
---

As I mentioned on Twitter the other day, I have this rather ugly spreadsheet that comes from some online survey software that requires quite a lot of cleaning in order to upload it to the database. I had an old version written in base R but the survey has changed so I’ve updated it to tidyverse.

And this is where tidyverse absolutely shines. Using it for this job really made me realise how much help it gives you when you’ve got a big mess and you want to rapidly turn it into a proper dataset, renaming, recoding, and generally cleaning.

It must be half the keystrokes or even less than the base R script it replaces. There are some quite long strings in there, which come from the survey spreadsheet, but that’s all just cut and paste, I didn’t write anything for them.

I love it profoundly, and I bet if I was more experienced I could cut it down even more. Anyway, here it is for your idle curiosity, it is obviously of no use to anybody who isn’t working on this data and to be honest there are bits that you probably won’t even understand, but just a quick look should show you just how much I had from the wonderful tidyverse maintainers

<https://gist.github.com/ChrisBeeley/488c3a8fa35b57d8b40232d70e1dfdc9>