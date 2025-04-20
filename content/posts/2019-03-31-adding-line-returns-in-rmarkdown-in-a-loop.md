---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-03-31T16:48:05Z"
title: Adding line returns in RMarkdown in a loop
---

Another one that’s for me when I forget. The internet seems strangely reluctant to tell me how to do this, yet [here it is](https://stackoverflow.com/questions/49561077/creating-a-new-line-within-an-rmarkdown-chunk) buried in the answer to something else.

Sometimes you are writing an RMarkdown document and wish to produce text with line returns between each piece. I can never work out how to do it. It’s very simple. Just two spaces and then \\n. Like this ” \\n”. Here’s some real code with it in.

```
  team_numbers %>% 
    mutate(print = paste0(TeamN, TeamC, "  \n  \n")) %>% 
    pull(print) %>% 
    cat()

```

Simple!