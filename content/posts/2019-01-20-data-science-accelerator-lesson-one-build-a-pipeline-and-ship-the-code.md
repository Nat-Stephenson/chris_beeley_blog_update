---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-01-20T17:41:39Z"
title: Data science accelerator lesson one- build a pipeline and ship the code!
---

My exciting news is that I was accepted onto the [data science accelerator](https://www.gov.uk/government/publications/data-science-accelerator-programme/introduction-to-the-data-science-accelerator) and have been doing it since late December. My project, basically, is all about using natural language processing to better understand the patient experience data that we collect (and, if I have time, the staff experience data too). Here are the goals:

1) Using an unsupervised technique, generate a novel way of categorising the text data to give us a different perspective. We already have tagged data but I would like to interrogate the usefulness of the tags that we have used  
    1) Generate a system that, given a comment or set of comments, can find other comments within the data that are semantically similar. Note that this system will need to run live on the server, since it would be impossible to store the semantic similarity of every comment to every other comment  
    2) Generate a system that, instead of searching by word, searches by similarity to that word  
3) Produce a supervised learning algorithm which can be trained on a sample of tagged comments and then produce tags for comments that it has not previously seen  
    1) Produce a sentiment analysis function that can tag every comment in a database with how positive or negative it is  
    2) Produce reporting functions that can compute overall sentiment for a group of documents (e.g. of a particular service area) and optionally describe the change in sentiment over time

I’m not really sure if I’m going to get through all of it but I’ve made a decent start. I’ve made a [Trello board](https://trello.com/b/GlmtpsqB/data-science-accelerator) and there’s a [GitHub](https://github.com/ChrisBeeley/naturallanguageprocessing) too.

One of the things about the project I haven’t mentioned above is that I want to make something that can easily be picked up and used by other Trusts. There are loads of companies who want to charge money for NHS Trusts to use their black box but I’m trying to make something others can use and build on. So a lot of the work at the end will be on that.

Anyway, I’ll share the work at the end but I’ve learned loads already so I thought I’d talk about that. It’s the best thing I’ve done since my PhD in terms of learning (so I recommend your doing it!) so there are lots of things I want to talk about.

The first one isn’t super complicated or a revelation to anyone but it’s affected the way I work already. It’s this. Ship code! Ship it! Get it out the door!

Up to now to be honest my agile working has pretty much been that I spend six months on something, I release it, it’s terrible, and then I make improvements. One of the product managers at GDS told me that they ship code every week. Every week! I couldn’t believe it. So I’m trying to work like that. Doesn’t matter if it isn’t perfect, doesn’t matter if some of the functionality is reduced, just get something in the hands of your users. Then if they hate it you can avoid spending a month building something they hate.

And, something related to that, start with a pipeline. Right at the start of an analysis, start building the outputs. This helps you to know what all this analysis is actually going to do. It helps you to make the analysis better. And it gives you code that you can ship. Build something that works and does something and give it to your users. They will start giving you feedback before you’ve even finished the analysis. Too often we start with the analysis and only think about the endpoint when we’ve finished. Maybe it’s the wrong analysis. Maybe what you’re doing is clever but no-one cares. Build a pipeline and get it out the door. Let your users tell you what they want.

More on this as the project proceeds