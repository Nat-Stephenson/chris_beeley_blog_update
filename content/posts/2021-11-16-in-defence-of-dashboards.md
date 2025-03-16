---
author: chrisbeeley
categories:
- Uncategorized
date: "2021-11-16T22:19:27Z"
title: In defence of dashboards
---

I’ve had lively debates about dashboards with various people, including someone in my own team, and somebody on Twitter just mentioned that dashboards are often not used (this blog post will be my response to this tweet, not the first time I’ve answered a tweet with a blog and very on brand for me 😉).

I should acknowledge at the top that I’m The Dashboard Guy. I’ve written books about making dashboards in R. It’s my role in the data science team in which I sit. [Shiny](https://shiny.rstudio.com/) in [production](https://github.com/CDU-data-science-team/presentations/tree/main/2021-04-08%20Shiny%20in%20production). It’s my thing. So take all this with as much salt as you wish, I promise I won’t be offended.

People say that dashboards proliferate, and that nobody uses them. That is quite true in a lot of cases. I’d like to suggest why that is so, and talk about when people do use them.

The first thing to say is that in the NHS (which is where I have always and will always work) many staff are not engaged with data in general. They’re not engaged with data, they’re not engaged with analysis, they’re not engaged with reports, they’re just not engaged. [They see data as a punishment, a “test” they cannot win]({{< ref "2021-07-18-stop-punishing-people-with-data.md" >}}). They can’t see the point of it and it’s just a distraction at best and over-critical and unfair at worst.

So my first question would be what do you replace the dashboards with? What will they engage with? I became The Dashboard Guy because we used to make a 300 page report by hand every quarter. It took literal person weeks and every recipient was only interested in their four page chunk. It was ridiculously inefficient. But everybody did want their four page chunk. Dashboards are useful for data that people want to see.

The team and I have recently built a [classification algorithm for free text patient experience data](https://github.com/CDU-data-science-team/pxtextmining). There is no way on earth anybody could use that without a dashboard because there are thousands of points of data and you are typically only interested in a few hundred at a time. [So we built one](https://github.com/CDU-data-science-team/experiencesdashboard). If they are using the algorithm at all, they’re using the dashboard (or someone else’s, it’s open source so you can DIY the dashboard if you want). If they’re not, that means they’re not looking at what their patient experience data is about, or they’re reading a 4000 row spreadsheet (and I do know people who have done/ are doing that).

So I think dashboards are very useful when you have a large highly structured dataset where everyone wants their own bit, and I’ve deployed perhaps three or four that have certainly been used by the individuals who wanted that data.

But something else that I think they can be used for is putting data science tools in the hands of your users. I just [built a dashboard](https://github.com/ChrisBeeley/lda_explorer) that allows you to pick how many LDA topics you want to use, and then shows you:

- Term frequency for each topic in a graph
- Five example comments from each topic

(I should say, it’s very rough and unfinished, but you get the idea). The idea of this is to allow the person who is interested in the data to make the topic model work themselves without writing R code. In this particular project all the data hasn’t come in yet anyway so the dashboard will contain more data in the future. It may be that the first tranche of data contains four topics, and the final dataset six. Building a dashboard makes the enduser part of that decision making process.

In fact, I actually build them for myself. A few years ago I was fitting a lot of structured topic models and it was such a pain in the neck fiddling around with the code that I just made a dashboard for myself so I could just sit and play around with it. The analysis itself took days, tens of hours, and the dashboard took an afternoon (to be fair, I’m probably a little faster than the average person with Shiny at least, because I have a lot of experience with it).

If your users are not using your dashboards, ask yourself why that is so. If they aren’t engaged with data or don’t care about the metrics then you have a bigger problem than dashboards. Would they engage with a report? An email? A bullet point in a team meeting? Build what they want, not what you think they want or should want.

Dashboards give you an opportunity to devolve analysis to your users. I work with a lot of text data. The users are the experts, not me. I want them to be able to do all the stuff I do in code but in a browser- sort, filter, summarise, aggregate, and even tune models (do they want five topics or fifteen? Do they want an overinclusive multilabel classification algorithm that captures all of a theme, or a conservative one that only shows them the highlights?).

In my opinion the dashboard is the icing on the cake. The team and I are building a very large, complex dashboard summarising many types of data, in the hope of driving customers between the types (patient experience data users looking at staff sickness levels, clinical effectiveness data users looking at all-cause mortality). But the work is in uniting the data, in producing analytics that are meaningful and useful. I don’t think it’s terribly important whether it’s a dashboard or a report or just played over the PA every Monday morning (there’s an idea 😉). Engage people in data and analytics first, and then serve that need using all the tools at your disposal.