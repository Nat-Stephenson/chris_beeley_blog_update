---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-04-21T12:39:35Z"
guid: http://chrisbeeley.net/?p=957
id: 957
title: Quarters and modulo arithmetic
url: /?p=957
---

This is another post that’s mainly for my benefit when I inevitably forget. I’m working with dates in PHP which, unlike MySQL, does not have a built in quarter function for extracting the quarter from a year.

Even if it did, one would have to be very careful with it because quarters are actually defined differently in different countries. In the UK, where I am, April to June is the first quarter (and January to March the last). In other countries, including (I think) the US, the first quarter is January to March, and October to December the last.

With no quarter function, and the hypothetical threat of an unpredictable one, my next recourse is to divide the month by 3 in order to give me a quarter number. This is done very simply as:

```
<pre class="brush: php; title: ; notranslate" title="">

echo ceil(date("m") / 3);

```

This will return 1 when the month is 1 to 3 (January to March), 2 when it’s 4 to 6 (April to June) etc. But as I mentioned before UK quarters don’t work like this. We don’t have quarters in a sequence 1, 2, 3, 4. They go in a sequence like 4, 1, 2, 3. Now I could write code that deals with each of those cases individually, converting 1 to 4, 2 to 1, 3 to 2, etc., but I thought it was better to do it properly (and store up generalisability for the future) by converting the 1, 2, 3, 4 sequence with modulo arithmetic. I confess, I still can’t work out how to do this other than by trial and error but it definitely involves modulo 4 because the sequence is of period 4.

To convert 1, 2, 3, 4 to 4, 1, 2, 3 one need only do the following, where x is the input value 1, 2, 3, 4:

4 – (5 – x) %% 4

As far as I can figure it the first constant (4) tells the sequence how big it should be (4, 1, 2, 3 or 10, 7, 8, 9) and the second constant (5) tells the series where it should flip back to the start (4, 1, 2, 3 or 3, 4, 1, 2).

That’s all I know, I’m really busy and don’t have time to think about it any more. Hopefully this will help someone/ me in two years’ time.