---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-08-08T17:01:57Z"
guid: http://chrisbeeley.net/?p=682
id: 682
title: Open source vs. commercial off the shelf
url: /?p=682
---

A government department in Australia reports saving very large amounts of money by shunning paid for proprietary software and implementing their own version with R ([link to story](http://mobile.itnews.com.au/News/390687,what-immigration-did-with-just-1m-and-open-source-software.aspx)).

If you follow the link they have some comments with a person who works at SAS who naturally talks down the benefits of free and open source software such as R (but then they would, wouldn’t they?). They report “He said the true cost of the supposedly free software can actually be quite substantial, when human resources and maintenance of the software is factored in”.

I’m very interested in this story, of course, because I have done this exact thing, shunned paid software and gone Do-It-Yourself using R, producing all the reports on my Trust’s [feedback website](http://feedback.nottinghamshirehealthcare.nhs.uk/reports).

I don’t know about “quite substantial”, but this approach was definitely not free and I am currently employed full time by the Trust working on the database and the front end reporting. To the best of my knowledge a commercial off the shelf (COTS) version of what we do would probably cost very roughly what they spend on me (very roughly, to within one order of magnitude, there are a lot of options and they are difficult to compare). And a COTS product would be smoother and, in particular, would have been smoother on day one: I’ve worked part time on this project for five years and the early days were rough.

I’m just about willing to concede that, on the face of it, a COTS version would have been cheaper and better. However, I’d like to argue here that this is not the whole story. My organisation and the public sector in general have worked with all sorts of providers of all kinds of software and I think there are two main drawbacks to this approach.

Firstly, the software does what it says on the tin. It does not do anything else, nor will it unless you are lucky enough to suggest it to the developers and have them accept it and begin work. There is no way for you to make the software do anything else, ever, even if you had the ability, which an organisation buying COTS products often won’t, because of course the source code is all closed. I honestly don’t think this would have worked for the feedback website which I have worked on. It’s grown in five years from a few bits of data on some scrappy spreadsheets with relatively little organisational buy-in to a colossal database comprising three very separate data sources which is scrutinised at board level monthly and which reports all over the organisation and to the general public over the internet. The change in the visibility of the project is very much to do with health policy in England and Wales and an ever increasing focus on patient experience and perhaps very particularly on the [Francis enquiry](http://www.midstaffspublicinquiry.com/) which emphasised the importance of listening to patients as well as being transparent about outcomes and experiences, which I’m proud to say I think my organisation does very well.

Whatever the reason, we had no idea when we began where we were going, and I’ve been involved right from the start devising technical solutions to the myriad problems thrown up with data and reporting as the project has scaled up over time. I don’t think COTS software that we had bought three years ago would do what we want it to today and I’m not sure if COTS software exists today that does the things we want to do next. Maybe it does, I don’t know. I’m not motivated to find out, of course.

Secondly, and I think more importantly, organisations which buy COTS solutions don’t have any expertise inhouse about how they work, what they could be made to do, types of data that can and can’t be added, how difficult or easy different things are, or anything else. Although naturally companies often provide support contracts I feel that this is a poor substitute for having a living presence within the organisation. For myself personally, I have worked in the Trust for 12 years, as a nursing assistant, assistant psychologist, PhD student, evaluator/ analyst/ researcher, and now programmer, and I have lived and breathed the patient feedback work for 5 years. So I’m in the organisation all the time, looking for new opportunities to do the work better, getting feedback on the reporting, discussing the technical implications of adding new data sources (and advising on psychometrics, too, with my psychology PhD hat on), and a million other different things. Having knowledge of the Trust and knowledge of the technology in *the same person* is actually, in my view, a very powerful tool to ensure that the needs of service users, carers, clinicians, and managers are best implemented both in terms of the robustness of data and the information technology that will be used to gather and report on the data.

The Trust has supported me very well to do the things that I have done, but I’ve had to make it all up myself, I didn’t get any support or supervision with it, and my job does not really seem to fit into any of the boxes into which jobs in the NHS are put.

My ultimate vision would be for NHS Trusts across the country to come together to solve some of the technological problems that they are all faced with and to share these solutions with each other and, ultimately, the world (I’d make the licence non-profit, personally, but that’s just my politics, either way is fine). This is the ethos behind such projects as the wonderful [NHS hackday](http://nhshackday.com/).

That’s a hard problem, I admit, but it’s one with a glorious solution, and I certainly hope I can make a tiny contribution towards it over the next 30 years.