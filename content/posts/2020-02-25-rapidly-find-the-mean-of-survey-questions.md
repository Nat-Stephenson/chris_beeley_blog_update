---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-02-25T14:18:00Z"
title: Rapidly find the mean of survey questions
---

Following on from the last blog post, I’ve got quite a nice way of generating lots of means from a survey dataset. This one relies on the fact that I’m averaging questions that go 2.1, 2.2, 2.3, and 3.1, 3.2, 3.3, so I can look for all questions that start with “2.”, “3.”, etc.

```
library(tidyverse)

survey <- survey %>% bind_cols(
  
  map(paste0(as.character(2 : 9), "."), function(x) {
    
    return_value <- survey %>% 
      select(starts_with(x)) %>% 
      rowMeans(., na.rm = TRUE) %>% 
      as.data.frame()
    
    names(return_value) <- paste0("Question ", x)
    
    return(return_value)
  }) 
)
```

This returns the original dataset plus the averages of each question, labelled “Question 2”, “Question 3”, etc.