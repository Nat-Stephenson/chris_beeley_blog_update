---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-05-26T17:18:50Z"
guid: http://chrisbeeley.net/?p=1366
id: 1366
title: Productionising R at Nottinghamshire Healthcare
url: /?p=1366
---

I’m hopeful we’re moving into a bit of a new phase with using R in my Trust so I thought I’d outline the direction of travel, to see if it chimes with anyone else and just to keep people up to date about what we’re doing.

We’ve used Shiny for some years now, maybe 7, and we have applications behind the firewall and in the cloud which are well used by the staff who need them. We’ve also been building our skills (R and data ops, which is a whole other post) and I’m hopeful that we’re ready for the next phase, which I would call the “productionise” phase. There are two main tasks to achieve before we’ve really got the work that we’re doing nicely embedded in everyday practice at Nottinghamshire Healthcare.

The first thing we’re looking at at the moment is using the data warehouse better. We have a very large and well featured data warehouse and it does loads of really great stuff but we (and I’m the worst offender for this) are still relying too much on pulling stuff out of it to say, Excel, and then reading that into R and off we go with an analysis. It’s a lot of work getting it out and then you can’t really communicate with the data warehouse people easily about what you did because you’ve introduced this whole other layer on top messing around with Excel sheets and you can’t easily tell them what you did. So the first bit for me (and this is not revolutionary in any way, we’re just building up to something) is to use the {odbc} and {pool} package, with shiny, to directly interface with the data warehouse, do the analysis, deploy it as a Shiny application, and just have it living live on the data warehouse. It’s striking straight away that when you do that suddenly you’re talking the same language as your BI team and you can communicate to them what you’ve done and give them to tools to reimplement it themselves if they want to.

And the second bit we need to get right is productionising the Shiny applications themselves. I’ve been looking at the {golem} package to do this. Golem really appeals to me for two reasons. Firstly, because it provides a robust framework for modularising code. I’m starting to realise that I do write the same Shiny code over and over again. Something in particular that I have done a lot is write code that makes sense of a spreadsheet. Ultimately I would like to write a module that can take an incredibly messy spreadsheet and with a few clicks from the user tidies it up ready for further processing. And with the right level of modularity I could use that over and over again. I did start writing one a little while ago but I got distracted.

I’ve got a few projects coming up that exemplify the process and an abstract submitted at the [R in medicine conference](https://events.linuxfoundation.org/r-medicine/) so hopefully I’ll be back to say more about this all in due course with some real examples. In the meantime there’s some bare bones golem code [here](https://github.com/ChrisBeeley/shinySPC) and [here](https://github.com/CDU-data-science-team/healthcareSPC) but it’s early days so please don’t judge me 🙂