---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-03-23T13:04:45Z"
title: NHS data science and software licensing
---

I’m writing something about software licensing and IP in NHS data science projects at the moment. I don’t think I ever dreamed about doing this, but I’ve noticed that a lot of people working in data science and related fields are confused about some of the issues and I would like to produce a set of facts (and opinions) which are based on a thorough reading of the subject and share them with interested parties. It’s a big job but I thought I’d trail a bit of it here and there as I go. Here’s the summary at the end of the licences section.

The best software licence for a data science project will vary case by case, but there are some broad things to consider when choosing one. The most important decision to make is between permissive and copyleft licences. Permissive licences are useful when you want to maximise the impact of something and are not worried about what proprietary vendors might do with your code. Releasing code under, for example, an MIT licence allows everybody, including individuals using proprietary code, a chance to use the code under that licence.

Using a copyleft licence is useful when there is concern about what proprietary vendors might do with a piece of code. For example, vendors could use some functionality from an open source project to make their own product more appealing, and then use that functionality to sell their product to more customers, and then use that market leverage to help them acquire *vendor lock in*. Vendor lock in is the state in which using a company’s prodcuts ensures that you find it very difficult to move to another company’s products. An example might be using a proprietary statistical software package and saving data in its proprietary format, making it difficult to transfer that data to another piece of software. If proprietary software companies can use code to make the world worse in this way, then choosing a copyleft licence is an excellent way of sharing your code without allowing anybody to incorporate it into a proprietary codebase. Proprietary software companies are free to use copyleft licensed code, but are highly unlikely to do so since it means releasing all of the code that incorporates it.