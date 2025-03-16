---
author: chrisbeeley
categories:
- Uncategorized
date: "2013-03-20T14:36:08Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=371
id: 371
publicize_reach:
- a:2:{s:7:"twitter";a:1:{i:906009;i:50;}s:2:"wp";a:1:{i:0;i:2;}}
publicize_twitter_user:
- ChrisBeeley
title: Data in the NHS
url: /?p=371
---

I wrote a little piece for an information pack related to the [Patient Feedback Challenge](http://www.institute.nhs.uk/innovation/spread_and_adoption/nhs_patient_feedback_challenge.html) so I thought I may as well share it here. I’m a little rushed so I won’t hyperlink the references, apologies for this they are all at the bottom in plain text.

NHS data systems are, quite naturally, built around very strict data management systems which emphasise the security of data and ensuring accountability of data loss or misuse. This is right and proper. However, very frequently, clinicians, managers, auditors and evaluators do not need to access highly sensitive personal information. In certain cases, information can be shared freely over the internet, as with the data collected on patient experience within the Patient Feedback Challenge (survey responses, Patient Opinion stories, and specific extracts from the PALS database).

The skills and technologies to rapidly analyse and disseminate data of this kind where rights access is not an issue is presently lacking in many NHS settings. Data analytic solutions, where they exist, are often large and complex and are complex and expensive to set up and modify. Where solutions are brought in from private sector providers these tend to be inflexible and it can be difficult for an organisation to achieve the results they want using off the shelf technologies (for example, integrating and resharing different types of data, such as the survey, Patient Opinion, and PALS data involved within the Patient Feedback Challenge).

A further limitation in the data architecture of many NHS organisations is the tendency for data to exist in silos, with each department maintaining its own separate database with no capacity to combine or reshare any of the data. Often the existence and location of datasets is unknown to others working elsewhere in a Trust.

Another problem endemic in many NHS settings is Spreadsheet addiction (e.g. Burns, 2004). This has been given as a possible culprit in JP Morgan Chase’s having lost 2 billion dollars in May 2012 (from http://goo.gl/SSAMS):

‘James Kwak, associate professor at the University of Connecticut School of Law… noted some interesting facts in JP Morgan Chase’s post-mortem investigation of the losses. Specifically, that the Value at Risk (VaR) model that underpinned the hedging strategy:

“operated through a series of Excel spreadsheets, which had to be completed manually, by a process of copying and pasting data from one spreadsheet to another”, and “that it should be automated” but never was.’

The process of analysing and presenting Service User and Carer Experience survey data began in a similar fashion, with spreadsheets and macros being used to produce summary graphics, and reports manually cut and pasted together. Fortunately for us, before we lost 2 billion dollars this way there was a mishap with the process and it became obvious that the process needed to go from “should be automated” to “has been automated” in order to meet the reporting timescales. An incremental approach to automation was adopted, and it’s worth noting that three years ago the lead analyst on the survey had no programming experience whatsoever. Each quarter the survey reporting would demand another layer of complexity and the analyst would learn out of books and by accessing helpful online communities how to perform whatever new task was demanded. As with most technological movements, automation did not bring a reduction in workload but rather more sophisticated and better results, and within a few years reporting on survey data had gone from a task that was difficult for one person to perform quarterly alongside other tasks to a level of complexity that would demand more than one individual working full time for the whole of each quarter just to meet each quarterly timescale.

The patient feedback challenge has allowed us (with our friends at [Numiko](http://numiko.com/)) to clear the final hurdle and to remove the last vestiges of human effort from the regular reporting, which means that quarterly survey reports and custom extracts will be available 24 hours a day, 365 days a year to anyone in the world with an internet connection. Managers will be able to see the latest results from their area and anywhere else in the Trust at any point in the reporting cycle and service users and carers will be able to transparently query and scrutinise all of the patient feedback data that we collect.

Two principles have guided the work on data architecture and processing. Firstly, all the technologies that were used are simple, free, and open source. Using languages such as R and Python and reporting technologies such as HTML and LaTeX means that not only are there no costs associated with buying or licensing software but also that analysis and reporting technologies are simple to use straight away. In a process known as agile software development (Beck et al., 2001) all of the technologies that have been produced have been put to immediate use. Early on, automation was only possible of graphics and manual effort was required to put together the report and write textual summaries. Later graphics and text were both automated and only formatting and final edits were performed by a human being. With the completion of the Patient Feedback Challenge now the formatting can also be automated. But at each stage the results of the technology were being used live in order to meet the demands of the organisation. This has the twin benefit of giving an immediate payoff to investment in workforce costs developing the software as well as giving the opportunity for the technology to be refined in use. The results of each stage of development were obvious when each reporting cycle was complete and work has been undertaken throughout to better serve both the Involvement team who deliver the survey results as well as the clinicians, managers, service users, and carers who make use of them.

The second principle is that all of the work which has taken place within the survey analysis and reporting and the Patient Feedback Challenge has left a legacy not only of technologies that can be used by the organisation but also systems within the organisation and skills and experience within the workforce which allow this work to prosper and flourish. Everyone involved in the data side of the project has put in place new systems to better capture and share data and learned valuable skills which will live within the organisation allowing further development work as well as new projects to be undertaken. A key feature of the whole Patient Feedback Challenge has been the development of staff and volunteers and the work with data and reporting was no different in this respect.

References

Beck et al. (2001). Manifesto for Agile Software Development. Agile Alliance. Retrieved 20-8-2012

Burns, 2004. Spreadsheet addiction. http://www.burns-stat.com/pages/Tutor/spreadsheet\_addiction.html Retrieved 16-5-2012