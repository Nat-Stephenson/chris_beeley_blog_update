---
author: chrisbeeley
categories:
- Uncategorized
date: "2022-06-23T15:45:00Z"
title: Data science for dummies (Goldacre)
---

Building your own tools for data science is a pretty fundamental concept, and I think it’s fair to say that it’s totally alien to most NHS bosses. I shall henceforth be showing them this excellent section from the Goldacre report. It’s not about data science as such, it’s about TREs, but it illustrates the concept beautifully

> The data science team at the music-streaming service Spotify do innovative work with data that helps drive the usability and popularity of their subscription service. For example, they extract patterns in the listening behaviour across all their users, and then use this to provide individual users with tailored recommendations for other music they might enjoy. The Spotify data science team couldn’t buy, off the shelf, a data science environment specifically built to service the needs of “a global music streaming service”. They implemented standard off-the-shelf tools for a general purpose data science environment. Then, within that raw environment, they needed to build their own tools, analytic approaches, workflows, data preparation work, and so on. A new arrival in the Spotify data science team today will find modules of code, libraries and packages – some even with nice interactive interfaces – to help them find interesting new patterns in Spotify user data. Many of these tools will feel like part of the furniture, but they were all built by their predecessors in the Spotify data science environment. Furthermore, many of these tools will not have been built to a pre-determined specification, by software developers hired to do that work to order; rather, they will emerge from a team. A single analyst might painstakingly implement a one-off analysis; if it looks like the approach will have broader use, then a more experienced developer might offer to help package it up into a function or library, with good documentation; if it becomes a commonly used approach, they might work with other analysts to create an interactive tool
> 
> <cite>[Better, broader, safer: using health data for research and analysis](https://www.gov.uk/government/publications/better-broader-safer-using-health-data-for-research-and-analysis/better-broader-safer-using-health-data-for-research-and-analysis#trusted-research-environments)</cite>

(reproduced under the terms of the [Open Government Licence](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/))