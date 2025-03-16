---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-02-09T10:30:27Z"
guid: http://chrisbeeley.net/?p=1047
id: 1047
title: Analysing runs from the Polar web flow service
url: /?p=1047
---

Well, we’re still in New Year’s resolutions territory, so what better time to have a look at using R to analyse data collected from a run? For this analysis I have used the Polar Flow web service to download two attempts at the same [Parkrun](http://www.parkrun.org.uk/forestrec/), recorded on a Polar M600 (which I love, by the way, if you’re looking for a running/ smartwatch recommendation).

The background to the analysis is in the second of the two runs I thought I was doing really well and was going to crush my PB and it ended up being exactly the same as the previous run, in terms of total time taken, but with my heart rate a lot lower.

But I didn’t really feel like I wasn’t pushing myself hard enough, so I can’t really explain why my heart rate has dropped so much without a corresponding increase in performance. One possible explanation is I have moved from being bottlenecked by the performance of my cardiovascular system to being bottlenecked by the performance of my legs, but that these two bottlenecks are very similar in terms of where they cap my pace.

It was pretty fun having a look in R. Here’s a [link to the analysis](http://rpubs.com/Chris_Beeley/analysis5K) as it stands.

I thought I would look at my race strategy in terms of how fast I went at each point, reasoning that maybe I let myself down on the hills or the straights or something in the second attempt. However, as you can see the pace is absolutely identical the whole way in both runs. The heart rate, as you can see, is consistently lower in the second run, and it only creeps up at the end for the sprint finish (which makes me wonder if I really was pushing myself hard enough).

I need to do more analysis. My next idea is to look at the relationship between incline, heart rate, and pace (the route is pretty hilly so this is quite important).