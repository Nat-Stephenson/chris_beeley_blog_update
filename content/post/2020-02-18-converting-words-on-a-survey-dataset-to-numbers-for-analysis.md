---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-02-18T13:41:50Z"
guid: http://chrisbeeley.net/?p=1287
id: 1287
title: Converting words on a survey dataset to numbers for analysis
url: /?p=1287
---

As always, I have very little time for blogging (sorry) but I just came up with a neat way of converting “Strongly Agree”, “Always”, all that stuff that you get on survey based datasets into numbers ready for analysis. It’s automatic, so it will play havoc with any word based questions- analyse them in a separate script.

Here it is

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: r; title: ; notranslate" title="">
library(tidyverse)

survey <- map_df(survey, function(x) {
  
  if(sum(grepl("Agree", unlist(x), ignore.case = TRUE)) > 0) {
    
    return(case_when(
      x == "Strongly agree" ~ 10,
      x == "Agree" ~ 7.5,
      x == "Neither agree nor disagree" ~ 5,
      x == "Disagree" ~ 2.5,
      x == "Strongly disagree" ~ 0,
      TRUE ~ NA_real_
    ))
  } else if(sum(grepl("Always", unlist(x), ignore.case = TRUE)) > 0){
    
    return(case_when(
      x == "Always" ~ 10,
      x == "Often" ~ 7.5,
      x == "Sometimes" ~ 5,
      x == "Rarely" ~ 2.5,
      x == "Never" ~ 0,
      TRUE ~ NA_real_
    ))
  
  } else if(sum(grepl("Dissatisfied", unlist(x), ignore.case = TRUE)) > 0){
    
    return(case_when(
      x == "Very satisfied" ~ 10,
      x == "Satisfied" ~ 7.5,
      x == "Neither satisfied nor dissatisfied" ~ 5,
      x == "Dissatisfied" ~ 2.5,
      x == "Very dissatisfied" ~ 0,
      TRUE ~ NA_real_
    ))

  } else {
    
    return(x)
  }
})

```

</div>Glorious. R makes it too easy, really, I think, sometimes 🙂