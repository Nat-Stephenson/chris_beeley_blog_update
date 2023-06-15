---
author: chrisbeeley
categories:
- Uncategorized
date: "2013-06-28T14:25:59Z"
guid: http://chrisbeeley.net/?p=454
id: 454
title: Department for redundancy department results of staff survey questionnaire
url: /?p=454
---

I’ve just received the following in a local newsletter.

<figure aria-describedby="caption-attachment-455" class="wp-caption alignnone" id="attachment_455" style="width: 2592px">[![Survey results](http://chrisbeeley.net/wp-content/uploads/2013/06/2013-06-28-15.07.09.jpg)](http://chrisbeeley.net/wp-content/uploads/2013/06/2013-06-28-15.07.09.jpg)<figcaption class="wp-caption-text" id="caption-attachment-455">Figure 1: The first figure</figcaption></figure>

It’s reasonably clear, so it passes the first test, but really the decimal places are redundant, as are the frequency counts. Much nicer in a bar chart, of course:

```
<pre class="brush: r; title: ; notranslate" title="">

barplot(c(23.6, 24.2, 26.4, 20.2, 6.2),
    names.arg = c("Not at all", "Not entirely", "Ambivalent", "Somewhat", "Very"),
    col = heat.colors(5), ylab="%", main = "Happiness with new PDPR procedure")

```

<figure aria-describedby="caption-attachment-456" class="wp-caption alignnone" id="attachment_456" style="width: 640px">[![Survey bar chart](http://chrisbeeley.net/wp-content/uploads/2013/06/Survey.png)](http://chrisbeeley.net/wp-content/uploads/2013/06/Survey.png)<figcaption class="wp-caption-text" id="caption-attachment-456">Voila!</figcaption></figure>