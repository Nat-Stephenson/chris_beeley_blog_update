---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-02-17T15:46:33Z"
title: Citing R packages in RMarkdown
---

I haven’t really done much in the way of citing papers in the last couple of years, I’ve spent my time either messing around with Shiny servers or databases or writing RMarkdown reports- and being horribly ill, of course, haha!- see this blog, passim. Don’t worry, I’m as fit as a flea now 🙂

Anyway, I’ve been citing one or two papers today and I’ve found it very easy to cite both papers and R packages in RMarkdown, so I’m here to tell you not to ignore it because it’s too hard, just do it. Step one: add the bottom line of the following to your YAML header:

—  
title: “Validation of complexity scores in AMH”  
author: “Chris Beeley”  
date: “21 January 2019”  
output: html\_document  
bibliography: bibliography.bib  
—

Now create a file called bibliography.bib in the same directory as the RMarkdown. You can either download the .bib off journal websites, or use a program dedicated for it (I didn’t bother, I’m just writing a few sides, not a thesis), or you can even write your own pretty easily by just looking at the template for a similar type of reference and hand editing the field. Here’s my hand edited bit for a paper that I referenced which didn’t have any snazzy download .bib on the website:

@Article{Butler2005,  
 author = “Butler, J.”,  
 title = “Monitoring Community Mental Health Team Caseloads: a systematic audit of practitioner caseloads using a criterion based audit tool”,  
 journal = “Advancing Practice in Bedfordshire”,  
 year = “2005”,  
volume=”2″,  
number=”3″,  
pages=”95–105″  
}

You cite this in your RMarkdown using whatever identifier is on the top line, in this case I put the Butler2005 there, and in the RMarkdown you just write @Butler2005.

You can cite R by just typing citation() into the console and copying the .bib into your .bib file, and you can cite, say, the psych package by typing citation(“psych”).

Packages will look funny when you cite them because they’re usually referred to by name, not by author, so rewrite your reference like this:

with assistance from the psych \[-@psych2018\] package.

The minus excludes it from the actual text while still including it in the references at the end.

Just press “knit” and it will put the references in automatically, so you probably want to write

\## References

above the end line or something like that.

That’s it! I just learned it from scratch, did it, and wrote this blog post in 10 minutes so you really have no excuse now, do you 😉 ?