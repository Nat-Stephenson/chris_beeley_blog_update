---
author: chrisbeeley
categories:
- Uncategorized
date: "2013-10-19T10:47:41Z"
guid: http://chrisbeeley.net/?p=487
id: 487
title: Patient Opinion data made easy with R
url: /?p=487
---

As you may or may not be aware, I recently helped on the data side of [Nottinghamshire Healthcare NHS Trust’s](http://www.nottinghamshirehealthcare.nhs.uk/) feedback website which lives [here](http://feedback.nottinghamshirehealthcare.nhs.uk/reports/custom). Part of this work was collecting the data we receive from the Trust-wide patient survey and bringing it in with other sources of feedback, including stories from our friends over at [Patient Opinion](https://www.patientopinion.org.uk/).

For a long time I’ve queried their data using their API (documentation is [here](”https://www.patientopinion.org.uk/info/api-docs”)) and fiddled around with it a bit, but my XML was never quite up to doing a proper extract and getting the data into the same structure as the rest of our patient experience data. The job sort of got shelved under “A computer program could do this, nobody has time to write the computer program, maybe a human will do it” which to be honest is quite a large shelf groaning with the weight of a lot of stuff at the moment.

Anyway, we’re off to show off the website at a big meeting on Thursday and this has spurred me into action. I’ve finally managed to wrangle the data into shape using, what else, the mighty [R](http://www.cran.r-project.org/) with assistance from the [RCurl](http://cran.r-project.org/web/packages/RCurl/index.html) and [XML](http://cran.r-project.org/web/packages/XML/index.html)packages.

I confess coming from a background of not even really knowing what XML was it’s been a bit of a slog but I have got there now so I’m writing this post to help out future explorers who might face this exact problem (hey, every NHS Trust should use Patient Opinion and have an R guy wrangling the data, no question about that) or a similar one. I’m going to show you the dumb way (which I did first) and then the clever way.

Point number 1, which I have to say is quite a common point number 1, is this is a lot easier on Linux than it is on Windows. RCurl and XML both have various problems on Windows which I don’t understand sufficiently to explain, suffice to say if I’m doing this job I boot to Linux and you should too.

Point number 2 is you’re going to need an API key, just email Patient Opinion and they’ll give you one. Mine will show as “XXXXX” in the following code so I’m afraid you won’t be able to run the code as it stands.

These instructions are based on an Ubuntu 13.04/ Linux Mint 15 (I’m cheating on Ubuntu with Mint at the moment) distribution, your mileage may vary, check your package management systems, etc.

The first thing you want to do before you launch R is make sure that the OS is set up with the libraries you need to make the RCurl and XML packages. From a terminal this is just:

sudo apt-get install libxml2-dev

sudo apt-get install libcurl4-gnutls-dev

Assuming these commands run okay, launch R and type at the console:

```
<pre class="brush: r; title: ; notranslate" title="">

install.packages(c("RCurl", "XML"))

```

Assuming both the packages install okay you’re ready to go. Here’s the dumb way first, which I have to say is remarkably easy.

My code here includes the instruction:

```
<pre class="brush: r; title: ; notranslate" title="">

getURL("https://www.patientopinion.org.uk/api/v1/opinions?healthservice=rha&apikey=XXXXX&take=100").

```

This contains two things of interest, firstly the health service code, which in our case is “rha”. This returns just stories about my Trust. It’s pretty simple to figure yours out by looking at the search pages or if you can’t do that just ask Patient Opinion and I’m sure they’d be glad to let you know. The second is my API key, shown here as “XXXXX”. As I mentioned you will need to get your own API key and place it here.

The one final thing to mention is that the code as written here queries all of the stories that have ever been published every time. Because it’s only searching a small part of the tree it does not bother Patient Opinion’s server too much, and there’s only 900 stories at the moment so it doesn’t take too long. You can use the “skip” function within the API (see the documentation) to only download stories you don’t currently have. I haven’t done this because I will probably move this code to a server where the process won’t have write permissions, which would obviously break it, so it just seems like wasted effort. In time I will probably look at getting write permissions for the process on the server (which I don’t manage) and changing the code but it’s not really a priority.

```
<pre class="brush: r; title: ; notranslate" title="">

library(RCurl)
library(XML)

lappend <- function(lst, obj) { # this is a function I stole from somewhere years ago to stick list items together

  lst[[length(lst)+1]] <- obj ; return(lst)

}

mylist=list(xmlToDataFrame( # download the first 100 stories

  getURL("https://www.patientopinion.org.uk/api/v1/opinions?healthservice=rha&apikey=XXXXX&take=100")

))

skip = 100 # initialise a variable to download the next 100

result = NULL # this will be used to check for errors in the download, i.e. are there any stories left

while(class(result) != "try-error") { # while no errors, i.e. while API still has more stories

  result = try(
    xmlToDataFrame(getURL(paste("https://www.patientopinion.org.uk/api/v1/opinions?healthservice=rha&apikey=XXXX&skip=",
    skip, "&take=100", sep="")))
)

if (class(result) != "try-error") mylist=lappend(mylist, result)  # if there are stories, stick them onto the end of the list

  skip=skip+100 # get the next 100 stories next time

}

fine=do.call("rbind", mylist) # stick all the stories together in a dataframe

write.csv(fine, file="~/PO.csv") # export a spreadsheet

```

This is the dumb way, and will return a dataframe with all the stories in, as well as dates, responses, and some other stuff. For the dumb way it’s pretty good but the problem I had was that is stuck all the information about the service (the NACS code which is a unique identifier for each service) together with the other stuff about the service (what it’s called, where it is) together. Pulling the NACS code to match it with our database seemed a bit tricky, but more seriously whenever more than one service is mentioned in a story, the whole thing got stuck together, both NACS codes, addresses, everything. I really didn’t think I could handle these cases accurately. So this weekend I took the plunge and decided to parse the XML properly myself.

I didn’t want much really from all the data, just the story itself, the date, and the NACS code for matching purposes.

In order to make it work, I had to learn a little bit about XPath, which the XML package makes use of, and of which I have never previously heard. There are various tutorials on the internet, such as [here](http://www.tizag.com/xmlTutorial/xpathtutorial.php) and [here](http://manual.calibre-ebook.com/xpath.html). It seems like one of those very deep things but you can learn what you need for this task in half an hour.

Once you’ve got the hang of XPath, it’s just a case of downloading 100 stories, stepping through the XML to find the nodes you’re interested in using XPath and then sticking it all together in a dataframe. The problem I had at first is that there are 100 stories exactly, but slightly more NACS codes, because of the multiple services problem I mentioned above. I dealt with this by repeating each story as many times as there are unique NACS codes for that story. This makes the number of stories and NACS codes equal. You can then combine them into two columns of a spreadsheet, each story being identified with as many codes as were identified in that story. Using the same while loop as before gets all the stories off the API.

Finally I have another spreadsheet where I’ve made a lookup table between the NACS codes and our internal service codes we use on the survey. Again, you can either figure out the service codes from Patient Opinion by using the search function and seeing what comes back or just ask Patient Opinion nicely and I’m sure they’ll be glad to help. Merging the two brings back all the stories that match with our database.

Here’s the code:

```
<pre class="brush: r; title: ; notranslate" title="">

library(RCurl)
library(XML)

###################################
######## DATA READ FROM API #######
###################################

### set the initial value of skip and make an empty dataframe and empty XML to keep everything

skip = 0

myFrame = data.frame()

myXML=NULL

### for as long as the API returns results, parse XML, convert to dataframe, and stick together dataframes

while(class(myXML) != "try-error") {

  myXML = try(
    getURL(paste("https://www.patientopinion.org.uk/api/v1/opinions?healthservice=rha&apikey=XXXX&skip=",
    skip, "&take=100", sep=""))
  )

  nodes = xmlParse(myXML)

  ### note the use of *[local-name() = 'XXX']
  ### this is because I couldn't get the namespace working

  opinion = xpathApply(nodes, "//*[local-name() = 'Opinion']/*[local-name() = 'Body']", xmlValue)

  date = substr(unlist(xpathApply(nodes, "//*[local-name() = 'Opinion']/*[local-name() = 'dtSubmitted']", xmlValue)), 1, 10)

  service = xpathSApply(nodes, "//*[local-name() = 'NACS']", xmlValue)

  sizes = xpathSApply(nodes, "//*[local-name() = 'HealthServices']", xmlSize)

  ### this function exists because some stories have more than one NACS code.
  ### So what it does is repeats stories (using the size variable)
  ### to make it the same length as the NACS, meaning stories are
  ### doubled next to each unique NACS. Then you can cbind

  listMat = mapply(function(x, y, z){
  
    rep(c(x, z), y)

  }, opinion, sizes, date)

  finalMat = matrix(unlist(listMat), ncol=2, byrow=TRUE)

  colnames(finalMat) = c("Story", "Date")

  final = data.frame(finalMat, "NACS" = unlist(service))

  if (class(myXML) != "try-error") myFrame = rbind(myFrame, final)

  skip=skip+100

}

###################################
######## match up NACS codes ######
###################################

POLookup = read.csv("poLookup.csv")

test = merge(myFrame, POLookup, by = "NACS")

write.csv(test, file = "POFinal.csv")

```

That’s it! I managed to match about 700 out of 900 stories this way, some of them are matched to rather larger service areas than we would like but honestly I think that’s the data’s fault. I hope that having done this we will now try to set aside a bit of time to work with Patient Opinion to improve the way our services are classified (just to be clear, this task is completely on us and Patient Opinion have been very helpful throughout with this complicated and onerous task).