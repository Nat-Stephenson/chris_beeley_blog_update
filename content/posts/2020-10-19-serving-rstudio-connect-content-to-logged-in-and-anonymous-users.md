---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-10-19T17:16:21Z"
title: Serving RStudio Connect content to logged in and anonymous users
---

We have our [patient feedback dashboard](https://involve.nottshc.nhs.uk:8443/experience/) in the open where anyone can see what Nottinghamshire Healthcare NHS Trust’s patients are saying about us. Now I’ve got a Connect licence I thought perhaps I might build another version for our staff where I put stuff that we can’t share- for example the comments of people who click the “I do not wish to share this comment” box.

But I don’t want to build and maintain two versions, that would be hideous, and I was going to put a litte

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: r; title: ; notranslate" title="">
logged_in <- TRUE
```

</div>at the top of the one for staff and then just swap the files round but knowing me I’ll do it in a hurry one Friday and accidentally publish the logged in version to everybody. For the benefit of those who are new at this, and indeed for those who are not that new but feel the need to double check it actually works like they think it does before they design the whole system around it (like me), it’s pretty simple.

Just publish one and make people authenticate to it, and publish one in the open. Separate apps, separate links, same code. But add somewhere something like

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: r; title: ; notranslate" title="">
loggedIn <- reactive({
    
  if(isTruthy(session$user)){
      
    TRUE
  } else {
    FALSE
  }
})
```

</div>Success!