---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-10-03T06:22:03Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=133
id: 133
tags:
- patient survey
- R
- simulation
- sweave
title: Simulating data
url: /?p=133
---

I try to publish as much of my work as I can on the internet, where the people who own the data agree (my little bit of the website is [here](http://www.institutemh.org.uk/-about-us-/our-evaluation-service)) but very often I can’t publish the data because of issues with ongoing projects, other publications, various levels of confidentiality of data, and so on.

So I’ve decided to try to simulate the data whenever there is an issue with publication. It’s very easy and it’s an excellent way for me to be able to show people my work without any of the issues that come from showing real data. The code is below, it comes in two parts for the first set of variables because we fiddled with the questionnaire a bit after Time 8 in the dataset.

```
<pre class="brush: r; title: ; notranslate" title="">

mydatafirst[mydatafirst$Time>8,
  which(names(mydatafirst)=="Service") : which(names(mydatafirst)=="Therapist")] = 
    apply(mydatafirst[mydatafirst$Time>8, which(names(mydatafirst)=="Service") : which(names(mydatafirst)=="Therapist")],
      2, function (x) sample(x[!is.na(x)], length(mydatafirst$Service[mydatafirst$Time>8]), replace=TRUE))

mydatafirst[mydatafirst$Time<9,
  which(names(mydatafirst)=="Service"):which(names(mydatafirst)=="Therapist")] = 
    apply(mydatafirst[mydatafirst$Time<9,which(names(mydatafirst)=="Service") : 
      which(names(mydatafirst)=="Therapist")],
      2, function (x) sample(x[!is.na(x)], length(mydatafirst$Service[mydatafirst$Time<9]), replace=TRUE))

mydatafirst[c(which(names(mydatafirst)=="Imp1"), which(names(mydatafirst)=="Best1"))] =
  apply(mydatafirst[c(which(names(mydatafirst)=="Imp1"), which(names(mydatafirst)=="Best1"))], 2, function (x)
    sample(x[!is.na(x)], length(mydatafirst$Imp1), replace=TRUE))

```

Thanks to the magic of R, and in particular the Sweave and Brew packages, all I need to do is insert these four lines into the code, re-run the report, and I have a nicely simulated dataset. I must confess I didn’t use R to convert the comments to gibberish, it was easier to download them from [here](www.random.org), but if this website didn’t exist then I certainly could have used R to do this very easily.

Something else that R and Sweave are really helping me with at the moment is making it possible to start to analyse data and compile reports before the data comes in. Because Sweave will automatically put together the statistics and graphs for me as I go along, it frees me up to just work on the data, share the progress with people as it comes along, and then put together the final analysis when all the data is collected, without having to manually re-write all the statistics and copy and paste all the graphs all the way through. I’ll post about the usefulness of Sweave and how it helps with workflow another time.