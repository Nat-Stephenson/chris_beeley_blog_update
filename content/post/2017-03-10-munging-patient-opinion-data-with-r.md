---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-03-10T11:31:23Z"
guid: http://chrisbeeley.net/?p=904
id: 904
title: Munging Patient Opinion data with R
url: /?p=904
---

I’ve written a post about using Patient Opinion’s new API, focusing on how to download your data using R but also perhaps useful to those using other languages as a gentle introduction, which can be found [here](http://chrisbeeley.net/?p=886).

This post focuses on the specific data operations which I perform in R in order to get the data nicely formatted and stored on our server. This may be of interest to those using R who want to do something very similar, much of this code will probably work in a similar context.

The code shown checks for the most recent Patient Opinion data every night and, if there is new data that is not yet on the database, downloads and processes it and uploads it to our own database. In the past I had downloaded the whole set every night and simply wiped the previous version, but this is not possible now we are starting to add our own data codes to the Patient Opinion data (enriching the metadata, for example by improving the match to our service tree all the way to team level where possible).

First things first, we’ll be using the lappend function, which I [stole from StackExchange](http://stackoverflow.com/questions/2436688/append-an-object-to-a-list-in-r-in-amortized-constant-time) a very long time ago and use all the time. I’ve often thought of starting a campaign to get it included in base R but then I thought lots of people must have that idea about other functions and it probably really annoys the base R people, so I just define it whenever I need to use it. One day I will set up a proper [R starting environment](http://www.onthelambda.com/2014/09/17/fun-with-rprofile-and-customizing-r-startup/) and include it in there, I use R on a lot of machines and so have always been frightened to open that particular can of worms.

```
<pre class="brush: r; title: ; notranslate" title="">

library(httr)
library(RMySQL)

### lappend function

lappend <- function(lst, obj) {

  lst[[length(lst)+1]] <- obj

  return(lst)
}
```

Now we store our api key somewhere where we can get it again and query our database to see the date of the most recent Patient Opinion story. We will issue a dbDisconnect(mydb) right at the end after we have finished the upload. Note that we add one to the date because we only want stories that are more recent than the database, and the API searches on dates after and *including* the given date.

We need to convert this date into a string like the following “01%2F08%2F2015” because this is the format the API accepts dates in. This date is the 1st August 2015.

```
<pre class="brush: r; title: ; notranslate" title="">

apiKey = "SUBSCRIPTION_KEY qwertyuiop"

# connect to database

theDriver <- dbDriver("MySQL")

mydb = dbConnect(theDriver, user = "myusername", password = "mypassword",
  dbname = "dbName", host = "localhost")

dateQuery = as.Date(dbGetQuery(mydb, "SELECT MAX(Date) FROM POFinal")[, 1]) + 1

dateFinal = paste0(substr(dateQuery, 6, 7), "%2F", substr(dateQuery, 9, 10), "%2F", substr(dateQuery, 1, 4))
```

Now it’s time to fetch the data from Patient Opinion and process it. We will produce an empty list and then add 100 stories to it (the maximum number the API will return from a single request), using a loop to take the next hundred, and the next hundred, until there are no more stories (it’s highly unlikely that there would be over 100 stories just in one day, obviously, but no harm in making the code cope with such an eventuality).

The API accepts a value of skip which allows you to select which story you want, in order of appearance, so this is set at 0, and then 100, and then 200, and so on within a loop to fetch all of the data.

```
<pre class="brush: r; title: ; notranslate" title="">

# produce empty list to lappend

opinionList = list()

# set skip at 0 and make continue TRUE until no stories are returned

skip = 0

continue = TRUE

while(continue){

  opinions = GET(paste0("https://www.patientopinion.org.uk/api/v2/opinions?take=100&skip=",
  skip, "&submittedonafter=", dateFinal),
  add_headers(Authorization = apiKey))

  if(length(content(opinions)) == 0){ # if there are no stories then stop

  continue = FALSE
  }

  opinionList = c(opinionList, content(opinions)) # add the stories to the list
  # increase skip, and repeat

  skip = skip + 100

}
```

Having done this we test to see if there are any stories since yesterday. If there are, the length of opinionList will be greater than 0 (that is, length(opinionList) &gt; 0). If there are we extract the relevant bits we want from each list ready to build it into a dataframe

```
<pre class="brush: r; title: ; notranslate" title="">
# if there are no new stories just miss this entire bit out

if(length(opinionList) > 0){

  keyID = lapply(opinionList, "[[", "id")
  title = lapply(opinionList, "[[", "title")
  story = lapply(opinionList, "[[", "body")
  date = lapply(opinionList, "[[", "dateOfPublication")
  criticality = lapply(opinionList, "[[", "criticality")
```

The service from which each story originates is slightly buried inside opinionList, and you’ll need to step through with lapply, extracting the $links element, and then take the object that results and step through it, taking the 5th list element and extracting the element named “id”. This actually makes more sense to explain in code:

```
<pre class="brush: r; title: ; notranslate" title="">
# location is inside $links

linkList = lapply(opinionList, "[[", "links")
location = lapply(linkList, function(x) x[[5]][['id']])

# Some general tidying up and we're ready to put in a dataframe

# there are null values in some of these, need converting to NA
# before they're put in the dataframe

date[sapply(date, is.null)] = NA
criticality[sapply(criticality, is.null)] = NA

finalData = data.frame("keyID" = unlist(keyID),
  "Title" = unlist(title),
  "PO" = unlist(story),
  "Date" = as.Date(substr(unlist(date), 1, 10)),
  "criticality" = unlist(criticality),
  "location" = unlist(location),
  stringsAsFactors = FALSE
)
```

Now we have the data in a dataframe it’s time to match up the location codes that Patient Opinion use with the codes that we use internally. There’s nothing earth shattering in the code so I won’t laboriously explain it all, there may be a few lines in it that could help you with your own data.

```
<pre class="brush: r; title: ; notranslate" title="">
### match up NACS codes

POLookup = read.csv("poLookup.csv", stringsAsFactors = FALSE)

### some of the cases are inconsistent, make them all lower case

finalData$location = tolower(finalData$location)

POLookup$NACS = tolower(POLookup$NACS)

PO1 = merge(finalData, POLookup, by.x = "location", by.y = "NACS", all.x = TRUE)

### clean the data, removing HTML tags

PO1$PO = gsub("<(.|\n)*?>", "", PO1$PO)

PO1$Date = as.Date(PO1$Date)

poQuarters = as.numeric(substr(quarters(PO1$Date), 2, 2))

poYears = as.numeric(format(PO1$Date, "%Y"))

PO1$Time = poQuarters + (poYears - 2009) * 4 - 1

### write PO to database

# all team codes must be present

PO1 = PO1[!is.na(PO1$TeamC), ]

# strip out line returns

PO1$PO = gsub(pattern = "\r", replacement = "", x = PO1$PO)
```

The following line is perhaps the most interesting. There are some smileys in our data, encoded in UTF-8, and I don’t know exactly why but RMySQL is not playing nicely with them. I’m sure there is a clever way to sort this problem but I have a lot of other work to do so I confess I just destroyed them as follows

```
<pre class="brush: r; title: ; notranslate" title="">
# strip out the UTF-8 smileys

PO1$PO = iconv(PO1$PO, "ASCII", "UTF-8", sub = "")
```

And with all that done we can use dbWriteTable to upload the data to the database

```
<pre class="brush: r; title: ; notranslate" title="">
# write to database

dbWriteTable(mydb, "POFinal", PO1[, c("keyID", "Title", "PO", "Date", "Time",
             "TeamC")], append = TRUE, row.names = FALSE)
}
```

That’s it! All done