---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-02-22T13:05:51Z"
guid: http://chrisbeeley.net/?p=1057
id: 1057
title: One weird trick to getting column types right with read_csv
url: /?p=1057
---

Using read\_csv from the tidyverse is so easy that I didn’t bother to look at the [readr documentation](https://cran.r-project.org/web/packages/readr/README.html) for a long time. However, I’m glad I did, because there is, as they say in the click bait world, one weird trick to get your column types right with read\_csv. read\_csv (or the other delimited file reading functions like read\_tsv) does a brilliant job guessing what column types things are but by default it only looks at 1000 rows. Fine for most datasets, but actually I have more than one dataset where the first 1000 rows are missing, which doesn’t help the parser at all. So do it manually and get it right. But what a pain, all that typing, right? Wrong. Just do this:

```
<pre class="brush: r; title: ; notranslate" title="">

testSpec = read_csv("masterTest.csv")

```

And you’ll get this output automatically:

```
<pre class="brush: r; title: ; notranslate" title="">

Parsed with column specification:
cols(
  TeamN = col_character(),
  Time = col_integer(),
  TeamC = col_double(),
  Division = col_integer(),
  Directorate = col_integer(),
  Contacts = col_integer(),
  HIS = col_character(),
  Inpatient = col_character(),
  District = col_character(),
  SubDistrict = col_character(),
  fftCategory = col_character()
)

```

You’re supposed to copy and paste that into a new call, putting right any mistakes. And in fact there is one, in this very spreadsheet, the parser incorrectly guesses that Inpatient is character when it is in fact integer- because the first 1000 rows are missing.

So just copy all that into a new call and fix the mistake, like this:

```
<pre class="brush: r; title: ; notranslate" title="">

testSpec = read_csv("masterTest.csv", 
                    col_types = 
                      cols(TeamN = col_character(),
                           Time = col_integer(),
                           TeamC = col_double(),
                           Division = col_integer(),
                           Directorate = col_integer(),
                           Contacts = col_integer(),
                           HIS = col_character(),
                           Inpatient = col_integer(),
                           District = col_character(),
                           SubDistrict = col_character(),
                           fftCategory = col_character()
                      ))

```

If you’re still having problems, you can have a look using problems(testSpec).

Absolute pure genius. The more I use the tidyverse, the more I know about it, and the more I know about it, the more I love it.