---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-09-12T16:54:30Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=108
id: 108
title: Robin Hood marathon results
url: /?p=108
---

I ran the Robin Hood marathon yesterday in a decent-ish 4 hours and 13 minutes, which is my best yet. Naturally, I was curious to see how my fellow runners fared, and so I have scraped the times from a pdf and summarised them using R and ggplot2.

I ran to support the <a></a>Disaster Emergency Committee, because of the East Africa Appeal, so if you would like to support this very worthy cause then please go [here](http://www.justgiving.com/Chris-Beeley).

Data scraping, for those that do not know, is the process of taking human-readable files like pdfs and webpages and turning them into computer-readable files like spreadsheets (more [here](http://en.wikipedia.org/wiki/Data_scraping)). The scraping itself was very simple since the [pdf](http://www.experianfestivalofrunning.co.uk/RobinHoodResults11/FullMarathon11v1.pdf) copy-pasted very nicely into a spreadsheet, which then read into R as a one variable list like so:

1 10038 Carl Allwood M Sutton &amp; Ashfield Harriers 02:38:40 1 02:38:40  
2 10098 Adam Holland M Votwo/USN 02:41:25 2 02:41:25  
3 13007 Pumlani Bangani M 02:43:23 3 02:43:23  
4 10028 Anthony Jackson M Sittingbourne Striders 02:44:39 4 02:44:39  
5 10187 Peter Stockdale M 02:45:26 5 02:45:25

The trick was merely to split up these big long strings and separate them into the correct variables, which, reading across, are:

Gun position (i.e. official position), race number, Name, Gender, Athletics club, Gun time (i.e. official time), Chip position and Chip time.

Chip position and chip time are the “real” time for slowcoaches such as myself, since it can take up to 10 minutes for all 15,000 runners to cross the line after the gun has gone- a chip therefore reads the time as you cross the start and finish line.

Code is at the bottom for those who are interested (and I would like to acknowledge the author of [this](http://mcfromnz.wordpress.com/2011/09/07/2011-perth-city-to-surf-stats/) post from whom I stole the “seconds” function to convert the times into a numeric format).

All times for finishers is shown below. My own time is represented by a vertical red line, with the median time being a black and dashed vertical line.

[![](http://chrisbeeley.net/wp-content/uploads/2011/09/rplot.png?w=300 "Rplot")](http://chrisbeeley.net/wp-content/uploads/2011/09/rplot.png)

Next up is the difference between male and female finishers, with medians for each group given as vertical lines.

[![](http://chrisbeeley.net/wp-content/uploads/2011/09/rplot012.png?w=300 "Rplot01")](http://chrisbeeley.net/wp-content/uploads/2011/09/rplot012.png)

And lastly, a faceted plot showing the differences between different ages and genders. I recoded some of the age categories because they vary across genders which makes a mess of the faceting.

[![](http://chrisbeeley.net/wp-content/uploads/2011/09/rplot02.png?w=300 "Rplot02")](http://chrisbeeley.net/wp-content/uploads/2011/09/rplot02.png)

The code:

```
<pre class="brush: r; title: ; notranslate" title="">
library(ggplot2)
library(car)

mydata=read.csv("D:\Dropbox\R-files\Marathon\Marathon_times.csv", stringsAsFactors=FALSE)

mylist1=strsplit(mydata$Var, "")

# find position, name and gender for all rows

mydata$Gunpos=lapply(mylist1, function(x) x[1])
mydata$Name=lapply(mylist1, function(x) x[3:4])
mydata$Gender=lapply(mylist1, function(x) x[5])

mydata$Chiptime=lapply(mylist1, function(x) x[length(x)])
mydata$Chippos=lapply(mylist1, function(x) x[length(x)-1])
mydata$Guntime=lapply(mylist1, function(x) x[length(x)-2])

# find the rows where the age category is included, i.e. 6th column is numeric

mydata$Age=NA

myvec=unlist(lapply(mylist1, function(x) as.numeric(x[6])&gt;0))
myvec[is.na(myvec)]=FALSE

mydata$Age[myvec]=unlist(lapply(mylist1, function(x) x[6])[myvec])

mydata$Age[is.na(mydata$Age)]=18

str(mydata$Age)
mydata$Age=factor(mydata$Age)

mydata$Age2=recode(mydata$Age, "'18'='18+'; c('35', '40')='35+'; c('45', '50')='45+'; c('55', '60')='55+'; c('65', '70', '75')='65+'";)

# fix the people with 3 names whose columns are misaligned

mydata$Gender[mydata$Gender!="M" & mydata$Gender!="F"]=lapply(mylist1, function(x) x[6])[mydata$Gender!="M" & mydata$Gender!="F"]

# fix the three stragglers with four names

mydata$Gender[c(331, 422, 1043)]="M"

# make gender a nicely formatted factor

mydata$Gender=factor(unlist(mydata$Gender))

# the title snuck in at row 75, delete this

mydata=data.frame(mydata[-75,])

### format the time values

# function from http://mcfromnz.wordpress.com/2011/09/07/2011-perth-city-to-surf-stats/

seconds <- function(x){
  as.numeric(substr(x,1,2)) * 60 * 60 +
  as.numeric(substr(x,4,5)) * 60 +
  as.numeric(substr(x,7,8)) 
}

mydata$Finaltime=seconds(unlist(mydata$Chiptime))/3600

### summarise

# overall
ggplot(mydata, aes(Finaltime)) + 
   labs(colour = "Gender") + 
   geom_density(size=1)+
   xlim(2, 7.5)+ 
   geom_vline(xintercept = 4+13/60, col="red", lty=1) +
   geom_vline(xintercept = median(mydata$Finaltime), col="black", lty=2)
   

# by gender
ggplot(mydata, aes(Finaltime, colour = droplevels(Gender))) + 
   labs(colour = "Gender") + 
   geom_density(size=1) +
   xlim(2, 7.5)+
   geom_vline(xintercept = median(subset(mydata, Gender=="F")$Finaltime), col="red", lty=1) +
   geom_vline(xintercept = median(subset(mydata, Gender=="M")$Finaltime), col="blue", lty=1)

# by age category and gender
ggplot(mydata, aes(Finaltime, colour = droplevels(Gender))) + 
   labs(colour = "Gender") + 
   geom_density(size=1) + facet_wrap(~Age2)+
   xlim(2, 7.5)
```