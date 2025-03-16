---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-10-24T11:45:15Z"
guid: http://chrisbeeley.net/?p=703
id: 703
title: All hail the mighty Cron- setting up cron jobs and a mail server to collect
  their output logs
url: /?p=703
---

At a certain point, everyone with a penchant for databases and Linux is going to want to make use of [cron](https://help.ubuntu.com/community/CronHowto), whether it’s to back up their data, run maintenance on certain fields of a database, or something else (in my case, do a huge pull of MySQL tables and format ready to drop nicely into a [Shiny](http://shiny.rstudio.com/) application).

Cron is, quite simply, a little pixie who lives on your server and executes scripts at times you determine: every day at midnight, every Wednesday morning, every 6 hours on weekdays, you name it, cron does it.

The internet is, as ever, generous with its advice, but I’ve read many web pages on my journey to getting the whole thing set up correctly so I thought I would pull some of the links and the advice into one post to help out other newbies such as myself.

My cron job is an R script, and [this](http://tgmstat.wordpress.com/2013/09/11/schedule-rscript-with-cron/) page gives a nice intro to running R scripts over cron, and also includes [this](http://www.thegeekstuff.com/2009/06/15-practical-crontab-examples/) link which gives loads of examples of customising when you want the scripts to run.

This should get you nicely on your way, but the foible of cron I had problems with is that it sends output and error messages by email, not by writing into a log. So when your script doesn’t work (which mine didn’t) it’s hard to know why. The first thing to know is that you can at least make sure the process is actually running, because there is a log of events, it just doesn’t have any output in it. As I discovered [here](http://askubuntu.com/questions/56683/where-is-the-cron-crontab-log), you can run:

```
<pre class="brush: bash; title: ; notranslate" title="">

grep CRON /var/log/syslog

```

This will give you a list of cron activity, most likely with “(CRON) info (No MTA installed, discarding output)” in it (on Ubuntu, anyway). What this means is there is no mail server and so the output (with those lovely error messages) is just trashed.

“I’m not setting up a whole mail server just to read some error messages”, I thought to myself, however, as time wore on and it became apparent that I couldn’t debug the script without seeing the output I thought I would have a go. It’s actually very easy to set up a simple mail server for this purpose. There are various bits and pieces of advice [here](http://askubuntu.com/questions/2261/how-are-administrators-supposed-to-read-roots-mail) but in essence you just need to run:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install postfix

```

In the configuration screen select “Local only”. In my case, that’s all I needed to do. Now to read your mail just

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install mailutils

```

And then run

```
<pre class="brush: bash; title: ; notranslate" title="">

mail

```

That’s it! I could now read the output and I found the error message, which came about because the working directory differed between running the script straight off the server myself and from a cron job. I don’t really know why, but I don’t care at this point, it works, that will do for today.