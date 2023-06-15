---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-03-19T18:56:03Z"
guid: https://chrisbeeley.net/?p=1209
id: 1209
title: Producing RMarkdown reports with Plumber
url: /?p=1209
---

I wasn’t going to post this until I got it working on the server but I’ve got the wrong train ticket and am stuck in London St Pancras until 7pm so I thought I’d be productive and put it up now.

So I launched a new version of the patient experience dashboard. I forget if I mentioned it on here or not. I think not. It’s here <http://109.74.194.173:8080/apps/SUCE/> and the code is here <https://github.com/ChrisBeeley/patient-experience-dashboard>. (I should add that I’ve launched a bit early because of an event so it will be a little buggy- forgive me for that).

One of the things we’ve done is get rid of the static reports that we used to host on the website, which we’re just generated as HTML, uploaded to the CMS and left there. We have preferred instead to switch to a dynamic reporting system which can generate different reports from a fresh set of data each time (I’ve barely started to implement different reports, so it’s very bare bones at the moment, but that’s the idea, at least).

One of my users, however, liked the way that the old reports would have links to all the team reports on. So she would send out one weblink to service managers and it would have all of the different teams on it and they could just click on each one as they wished. So I need to replicate this somehow. My thinking has gone all round the houses so I’ll spare you the details but basically I thought originally that I could do this with Shiny, generating URLs in each report that would contain query strings that could then be fed into the report generator, which means that clicking each link would bring back each team report. I can’t say for definite that it’s impossible, but I couldn’t figure out a way to do it because Shiny downloads are much more built around the idea that your user clicks a button to get the report. I could have had the link generate a Shiny application with a button in that you press to get the team report, but that’s two clicks and seems a bit clunky. Really, Shiny is not the correct way to solve this problem.

So I hit upon the idea of doing it with Plumber (<https://www.rplumber.io/>). There are very few examples that I could find on the internet of using Plumber to generate parameterised reports with RMarkdown, and none at all generating Word documents (which everyone here prefers) so I’m offering this to help out the next person who wants to do this.

Just getting any old RMarkdown to render to HTML is pretty easy. Have a look at the Plumber documentation, obviously, but basically you’re looking at

```
<pre class="brush: r; title: ; notranslate" title="">

#* @serializer contentType list(type="application/html")
#* @get /test
function(res){
  
  include_rmd("test_report.Rmd", res)
}
```

If you run this API on your machine it will be available at localhost:8000/test (or whatever port Plumber tells you it’s running on). You can see where the /test location is defined, just above the function.

Easy so far. I had two problems. The first one was including parameters. For all I know this is possible with the include\_rmd function but I couldn’t work it out so I found I had to use rmarkdown::render in the function. Like this:

```
<pre class="brush: r; title: ; notranslate" title="">

#* @serializer contentType list(type="application/html")
#* @get /html
function(team){
  tmp <- tempfile()
  
  render("team_quarterly.Rmd", tmp, output_format = "html_document",
         params = list(team = team))
  
  readBin(tmp, "raw", n=file.info(tmp)$size)
}
```

This API will be available on localhost:8000/html?team=301 (or whatever you want to set team to).

The RMarkdown just looks like this:

```
<pre class="brush: r; title: ; notranslate" title="">
---
title: Quarterly report
output: html_document
params:
  team: NA
---

`r params$team`

```

You can see you define the params in the YAML and then they’re available with params$team, which will be set to whatever your ?team=XXX search string is.

Okay, getting there now. The last headache I had was making a Word document. This is only difficult because I didn’t know to put the correct application type in the serializer bit on the first line that defines the API. You just need this:

```
<pre class="brush: r; title: ; notranslate" title="">

#* @serializer contentType list(type="application/vnd.openxmlformats-officedocument.wordprocessingml.document")
#* @get /word
function(team){
  tmp <- tempfile()
  
  render("team_quarterly.Rmd", tmp, output_format = "word_document",
         params = list(team = team))


  readBin(tmp, "raw", n=file.info(tmp)$size)
}

```

This will be available at localhost/word?team=301.

That’s it! Easy when you know how. All the code is on Git here [https://github.com/ChrisBeeley/reports\_with\_plumber](https://github.com/ChrisBeeley/reports_with_plumber).

I’m pretty excited about this. I did it to solve a very specific problem that I had for one of my dashboard users but it’s pretty obvious that having an API that can return reports parameterised by query string is going to be a very powerful and flexible tool in a lot of my work.

I’ll get it up on the server over the weekend. If that causes more headaches I guess I’ll be back with another blog post about it next week 🙂