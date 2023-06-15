---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-04-24T13:26:19Z"
guid: http://chrisbeeley.net/?p=530
id: 530
title: Useful R functions
url: /?p=530
---

Today seems as good a day as any to start archiving all the useful R functions I see scattered around the internet. And here seems as good a place as any to do it since I can see it and so can anyone else who is interested. Two to start, will add more as I get them. Credit to original authors, follow the link.

Removing whitespace

```
<pre class="brush: r; title: ; notranslate" title="">

trimws()

```

[Extract the last n characters of a string](http://stackoverflow.com/questions/7963898/extracting-the-last-n-characters-from-a-string-in-r)

```
<pre class="brush: r; title: ; notranslate" title="">

substrRight <- function(x, n){
  substr(x, nchar(x)-n+1, nchar(x))
}

```

I can’t credit this one, sadly, I can’t remember where I stole it from. It’s a nice summary of the [Anscombe](http://en.wikipedia.org/wiki/Anscombe's_quartet) data anyway. Sorry to whoever wrote it! I’ve used it many times, I’m grateful to you!

```
<pre class="brush: r; title: ; notranslate" title="">

library(ggplot2)
library(plyr)

anscombe2 <- with(anscombe, data.frame(
  x     = c(x1, x2, x3, x4),
  y     = c(y1, y2, y3, y4),
  group = gl(4, nrow(anscombe))))

(stats <- ddply(anscombe2, .(group), summarize, mean = mean(y),
   std_dev = sd(y), correlation = cor(x, y), lm_intercept = lm(y ~ x)$coefficients[1],
   lm_x_effect = lm(y ~ x)$coefficients[2]))


(p <- ggplot(anscombe2, aes(x, y)) +

  geom_point() +

  facet_wrap(~ group)
)

```

This one I always forget, and it’s so useful. Extract the nth element from a list, returning a list

```
<pre class="brush: r; title: ; notranslate" title="">

> splitThemes
[[1]]
[1] "Communication" "Other"        

[[2]]
[1] "Access to Services" "Other"             

[[3]]
[1] "Access to Services" "Waiting time"      

> lapply(splitThemes, "[", 2)
[[1]]
[1] "Other"

[[2]]
[1] "Other"

[[3]]
[1] "Waiting time"

```

On the subject of the “\[” function, look at this beauty I just found on [Stack Overflow](http://stackoverflow.com/questions/10883605/truncating-the-end-of-a-string-in-r-after-a-character-that-can-be-present-zero-o).

```
<pre class="brush: r; title: ; notranslate" title="">

x <- c('foobar','foo:bar','foo1:bar1 foo:bar','foo bar')
> sapply(str_split(x,":"),'[',1)
[1] "foobar"  "foo"     "foo1"    "foo bar"

```

[Read from clipboard](http://stackoverflow.com/questions/13438556/how-do-i-copy-and-paste-data-into-r)

There seem to be various issues with OS with this one, follow the link above for more. For me (Ubunutu Linux) it seems to work on some machines and not on others. I’m afraid I don’t know enough to figure out why and I’m repeatedly crashing RStudio trying to find out, so I’ve given in for now. I may come back to this when I’m less thick.

```
<pre class="brush: r; title: ; notranslate" title="">

copdat <- read.delim("clipboard")

```

If you can’t get it working there’s a lovely [package](https://cran.r-project.org/web/packages/clipr/index.html) that will do it for you (when isn’t there?).

[Strip HTML from a string](http://stackoverflow.com/questions/3765754/remove-html-tags-from-string-r-programming)

```
<pre class="brush: r; title: ; notranslate" title="">

gsub("<(.|\n)*?>","",string

```

[Read several spreadsheets from within one folder and stick them all together](http://psychwire.wordpress.com/2011/06/03/merge-all-files-in-a-directory-using-r-into-a-single-dataframe/)

I changed this one slightly from the place whence I stole it, so you may like to look at the original in the link above to see which is closer to what you are doing.

```
<pre class="brush: r; title: ; notranslate" title="">

file_list <- list.files("~/YOUR/PATH/HERE", full.names = TRUE)

dataset <- do.call("rbind", lapply(file_list, FUN = function(files){
  read.csv(files)
  }
))

```

[Change a vector of strings to title case](http://stackoverflow.com/questions/15776732/how-to-convert-a-vector-of-strings-to-title-case)

```
<pre class="brush: r; title: ; notranslate" title="">

strings = c("first phrase", "another phrase to convert",
             "and here's another one", "last-one")
gsub("\\b([a-z])([a-z]+)", "\\U\\1\\L\\2" ,strings, perl=TRUE)
## [1] "First Phrase"              "Another Phrase To Convert"
## [3] "And Here's Another One"    "Last-One" 

```

And we all want to [rbind dataframes with different number of columns occasionally](http://stackoverflow.com/questions/3402371/rbind-different-number-of-columns), here’s a cool function to do that.

```
<pre class="brush: r; title: ; notranslate" title="">

library(gtools)
df1 <- data.frame(a = c(1:5), b = c(6:10))
df2 <- data.frame(a = c(11:15), b = c(16:20), c = LETTERS[1:5])

smartbind(df1, df2)

# result
     a  b    c
1.1  1  6 <NA>
1.2  2  7 <NA>
1.3  3  8 <NA>
1.4  4  9 <NA>
1.5  5 10 <NA>
2.1 11 16    A
2.2 12 17    B
2.3 13 18    C
2.4 14 19    D
2.5 15 20    E

```

[Plotting a binomial outcome against a continuous variable](http://stats.stackexchange.com/questions/45444/how-do-you-visualize-binary-outcomes-versus-a-continuous-predictor)

```
<pre class="brush: r; title: ; notranslate" title="">

library(ggplot2) # plotting package for R

N=100
data=data.frame(Q=seq(N), Freq=runif(N,0,1), Success=sample(seq(0,1), 
size=N, replace=TRUE))

ggplot(data, aes(x=Freq, y=Success))+geom_point(size=2, alpha=0.4)+
  stat_smooth(method="loess", colour="blue", size=1.5)+
  xlab("Frequency")+
  ylab("Probability of Detection")+
  theme_bw()

```

[Report the memory usage of all the objects in the current session](http://stackoverflow.com/questions/1395270/determining-memory-usage-of-objects)

```
<pre class="brush: r; title: ; notranslate" title="">

sort(sapply(ls(), function(x){
    format(object.size(get(x)), units = "MB")
    }
  )
)

```

[Split a vector into equal n sized chunks](http://stackoverflow.com/questions/3318333/split-a-vector-into-chunks-in-r)

```
<pre class="brush: r; title: ; notranslate" title="">

chunk <- function(x,n) split(x, cut(seq_along(x), n, labels = FALSE))

```

[First day of the month](http://suehpro.blogspot.co.uk/2014/12/first-day-of-month-using-r.html)

```
<pre class="brush: r; title: ; notranslate" title="">

# get some dates for the toy example:
df1 <- data.frame(YourDate = as.Date("2012-01-01") + seq(from = 1,to = 900,by = 11))

df1$DayOne <- df1$YourDate - as.POSIXlt(df1$YourDate)$mday + 1

```