---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-08-01T14:55:47Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=8
id: 8
tags:
- data visualisation
- patient survey
title: 'The patient survey: past, present and future'
url: /?p=8
---

Within my organisation we have something known as the Service User and Carer Experience Survey, often abbreviated to the SUCE, or in more natural spoken English, the patient survey. It’s a chance for the users of our services to tell us about our services and includes Likert-type “ticky box” questions about specific aspects of care as well as an open section in which they can give an area for improvement as well as something they like about the service. Results are published quarterly and reporting represents a major challenge- we report over about 100 teams, all organised into larger directorates and divisions, with each category having its own type of report. I’ve been involved right from the start and the process goes nicely from a How-Not-To-Do-It guide to a How-To guide.

At first I was responsible for all data entry and reporting and in my naïveté I did everything manually. I made a page in Microsoft Excel which automatically generated frequency tables from raw data, and then fed those values into pie charts (yes I know: [juiceanalytics.com/writing/the-problem-with-pie-charts](http://www.juiceanalytics.com/writing/the-problem-with-pie-charts/) but I’m afraid many find them accessible, possibly due to familiarity? Or just overall “friendly” appearance?). I made the bar graphs in SPSS. The whole thing took ages and was done quarterly. The first step on the road to getting the computer to shoulder this massive burden was writing some R ([The R project](http://cran.r-project.org/)) code which produced the graphs. It was very easy to do, I was a real novice back then, and here it is:

```
<pre class="brush: r; title: ; notranslate" title="">

# select the current team, denoted x, and select the most recent mydata for the pie charts (put into Subset.P). Put data for the team from all points of time into Subset.B, which will be used to draw the barcharts 

Subset.P=subset(mymydata, mydata$Code==x & mydata$Time==8)
Subset.B=subset(mymydata, mydata$Code==x)

# add labels to each variable, and calculate the percentages in each category

Service.P=table(factor(Subset.P$Service, levels=1:6, labels=c("Very poor", "Poor", "Fair", "Good", "Very good", "Excellent")))
Service.P=Service.P/sum(Service.P)
names(Service.P) &lt;- paste(names(Service.P), "-", round(Service.P*100), "%", sep="")

# this bit fixes the colours and steps through each category removing the colours from the empty categories. Without this step the colours aren't consistent if there are any empty categories

attributes(Service.P)$cols <- rainbow(6)
    if(0 %in% Service.P){
        ind <- which(Service.P == 0)
        Service.P <- Service.P[-ind]
        attributes(Service.P)$cols <- rainbow(6)[-ind]
    }

# and so on for each question

Comm.P=table(factor(Subset.P$Communication, levels=1:6, labels=c("Very poor", "Poor", "Fair", "Good", "Very good", "Excellent")))
Comm.P=Comm.P/sum(Comm.P)
names(Comm.P) <- paste(names(Comm.P), "-", round(Comm.P*100), "%", sep="")
attributes(Comm.P)$cols <- rainbow(6)
    if(0 %in% Comm.P){
        ind <- which(Comm.P == 0)
        Comm.P <- Comm.P[-ind]
        attributes(Comm.P)$cols <- rainbow(6)[-ind]
}

#... I've edited the repetitions of the above out to save space

# draw all the charts in a 3 by 2 grid ready for copying straight into the report (the last graph is drawn later)

par(mfrow=c(3,2))

pie(Service.P, clockwise=TRUE, main = c("Service quality"), radius=1, cex=1.1, init.angle=45, col=attr(Service.P, "cols"))
pie(Comm.P, clockwise=TRUE, main = c("Communication"), radius=1, cex=1.3, init.angle=45, col=attr(Comm.P, "cols"))
pie(Ldign.P, clockwise=TRUE, main = c("Dignity and respect"), radius=1, cex=1.3, init.angle=45, col=attr(Ldign.P, "cols"))
pie(Involved.P, clockwise=TRUE, main = c("Involved in care"), radius=1, cex=1.3, init.angle=45, col=attr(Involved.P, "cols"))
pie(Improved.P, clockwise=TRUE, main = c("Improved life"), radius=1, cex=1.3, init.angle=45, col=attr(Improved.P, "cols"))

# take the Subset.B data, which contains all data for each team across each time point, as opposed to the Subset.P which is just this quarter's data for the pie charts

# also multiply the figures to fix them all at a maximum of 5

Subset.B$Service=Subset.B$Service*5/6
Subset.B$Communication=Subset.B$Communication*5/6
Subset.B$Ldign=Subset.B$Ldign*5/3
Subset.B$Involved=Subset.B$Involved*5/3

# produce a matrix, ybar, which will hold the responses for each
# question at 5 time points- last year and the next 4 quarters

ybar=as.data.frame(matrix(data=NA, nrow=5, ncol=5))
names(ybar)=c("Apr - Mar 10", "Apr - Jun 10", "Jul - Sept 10", "Oct - Dec 10", "Jan - Mar 11")

# construct each one from the mean at time < 5, time==6, time==7 etc.

ybar[1,] = c(mean(subset(Subset.B, Time<5)$Service, na.rm=TRUE),
             mean(subset(Subset.B, Time==5)$Service, na.rm=TRUE),
             mean(subset(Subset.B, Time==6)$Service, na.rm=TRUE),
             mean(subset(Subset.B, Time==7)$Service, na.rm=TRUE),
             mean(subset(Subset.B, Time==8)$Service, na.rm=TRUE))

ybar[2,] = c(mean(subset(Subset.B, Time,5)$Communication, na.rm=TRUE),
             mean(subset(Subset.B, Time==5)$Communication, na.rm=TRUE),
             mean(subset(Subset.B, Time==6)$Communication, na.rm=TRUE),
             mean(subset(Subset.B, Time==7)$Communication, na.rm=TRUE),
             mean(subset(Subset.B, Time==8)$Communication, na.rm=TRUE))

ybar[3,] = c(mean(subset(Subset.B, Time<5)$Ldign, na.rm=TRUE),
             mean(subset(Subset.B, Time==5)$Ldign, na.rm=TRUE),
             mean(subset(Subset.B, Time==6)$Ldign, na.rm=TRUE),
             mean(subset(Subset.B, Time==7)$Ldign, na.rm=TRUE),
             mean(subset(Subset.B, Time==8)$Ldign, na.rm=TRUE))

ybar[4,] = c(mean(subset(Subset.B, Time<5)$Involved, na.rm=TRUE),
             mean(subset(Subset.B, Time==5)$Involved, na.rm=TRUE),
             mean(subset(Subset.B, Time==6)$Involved, na.rm=TRUE),
             mean(subset(Subset.B, Time==7)$Involved, na.rm=TRUE),
             mean(subset(Subset.B, Time==8)$Involved, na.rm=TRUE))

ybar[5,] = c(mean(subset(Subset.B, Time&lt;5)$Improved, na.rm=TRUE),
             mean(subset(Subset.B, Time==5)$Improved, na.rm=TRUE),
             mean(subset(Subset.B, Time==6)$Improved, na.rm=TRUE),
             mean(subset(Subset.B, Time==7)$Improved, na.rm=TRUE),
             mean(subset(Subset.B, Time==8)$Improved, na.rm=TRUE))

# remove all the empty bits, to prevent the graph containing ugly holes

if(is.na(ybar[,1])|is.na(ybar[,2])|is.na(ybar[,3])|is.na(ybar[,4])){
ybar=ybar[-which(is.nan(ybar[1,])==TRUE)]
}

# draw the graph, the enormous ylim value is to give room at the top for the legend 

barplot(as.matrix(ybar), beside=T, col=rainbow(5), ylim=c(0,9), yaxt = "n")
axis(2, at = 0:5)
legend("top", legend=c("Service quality", "Communication", "Dignity and respect",
                       "Involved with care", "Improved life"), fill=rainbow(5), bty="n", cex=1, ncol = 2 )

# now manually change the value of x to go to the next team and re-run the code

```

This greatly sped up the process of producing the graphs, but left a lot of the process in the hands of a human- figuring out which teams were reporting that quarter, running the code, pasting the graphs, counting the responses, etc. etc. As I’ve grown more confident I’ve given more and more of these tasks to the computer and saved more and more time. I’ll show some more of the steps in subsequent posts, leading up to where we are now and plans for the future.