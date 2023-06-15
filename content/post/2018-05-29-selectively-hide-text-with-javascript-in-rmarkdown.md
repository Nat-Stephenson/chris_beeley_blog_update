---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-05-29T09:11:06Z"
guid: https://chrisbeeley.net/?p=1104
id: 1104
title: Selectively hide text with JavaScript in RMarkdown
url: /?p=1104
---

I guess this is one of those where I kind of did know it was possible, really. If I’d thought it through. But I’ve always not been sure how easy it would be and I’ve been in too much of a rush.

So I’m trying to move my organisation onto HTML instead of Word. HTML is easier to output and parse, and it’s interactive with a bit of JavaScript. I used the DT package in R to put some tables into an RMarkdown document and of course that’s very nice because they’re pageable, orderable, and searchable out of the box.

But there’s often a lot of text that I’m not sure everyone will be interested in. We collect a lot of comments and some people want every single one. Some people just want the tables. So it would be nice to be able to selectively show and hide the text in each section.

And I did know this, really. But RMarkdown documents accept pure HTML. And pure JavaScript. So it’s embarrassingly easy. Ridiculously easy. Here’s one. I half stole it from w3chools.com

```
<pre class="brush: xml; title: ; notranslate" title="">

---
title: "JavaScript test"
author: "Chris Beeley"
date: "11 May 2018"
output: html_document
---

<script>
function myFunction() {
    var x = document.getElementById("myDIV");
    if (x.style.display === "none") {
        x.style.display = "block";
    } else {
        x.style.display = "none";
    }
}
</script>

<button onclick="myFunction()">Show/ hide</button>

<div id="myDIV">

Some comments here.

There are lots so it's nice.

To be able to hide them.

</div>

```

That’s it. Boom. Done. If you’re not all doing that by the middle of next week then you need to be asking yourselves why.