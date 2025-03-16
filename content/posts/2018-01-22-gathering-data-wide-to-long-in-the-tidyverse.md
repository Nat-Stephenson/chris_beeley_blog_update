---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-01-22T16:27:34Z"
guid: http://chrisbeeley.net/?p=1042
id: 1042
title: Gathering data (wide to long) in the tidyverse
url: /?p=1042
---

I seem to have a pretty severe mental block about the gather() function from dplyr so this is yet another post that to be honest is basically for me to refer to in 6 months when I forget all this stuff. So I’m going to address the mental block I have very specifically and show some code; hopefully it will help someone else out there.

So whenever I use gather I put the whole dataframe in. Say I’ve got ten variables. I whack the whole dataframe in and try to pull out just the ones I want using the select() notation at the end of the list of arguments. This DOES NOT MAKE ANY SENSE. You can’t do this:

```
<pre class="brush: r; title: ; notranslate" title="">

theData = tibble(ID = 1:10, Q1 = runif(10),
  Q2 = runif(10),
  Q3 = runif(10),
  Q4 = runif(10),
  Q5 = runif(10))

gather(theData, key = Question, value = Score, Q1, Q2, Q3)

```

This does not work! I don’t know why I think it does! What do I think is going to happen to the ID column? It’s just going to magically go away?

I DON’T KNOW WHY I’M SO BAD AT THIS.

It’s going to gather the whole dataframe, and you just end up with a huge mess. The other thing to say is, and I have started to get the hang of this, but just in case. THE KEY AND VALUE ARGUMENTS YOU JUST MAKE UP. THEY ARE \*NOT\* RELATED TO THE NAMES OF THE DATAFRAME AT ALL.

What you actually do is get JUST THE VARIABLES YOU WANT, and then you need to decide whether you want any other variables, but not gather them. So as a concrete example, let’s say you want to gather Q1 – Q3 and keep the ID column. You want to put the ID column in, but you don’t want to GATHER it. So you put it in the select statement, but use -ID in the gather statement:

```
<pre class="brush: r; title: ; notranslate" title="">

testData %>%
  select(ID : Q3) %>%
  gather(key = Question, value = Score, -ID)

# A tibble: 30 x 3
  ID Question Score
  <int> <chr> <dbl>
 1 1 Q1 0.26001265
 2 2 Q1 0.34674771
 3 3 Q1 0.43080742
 4 4 Q1 0.28397929
 5 5 Q1 0.14545496
 6 6 Q1 0.63496928
 7 7 Q1 0.78777785
 8 8 Q1 0.44622476
 9 9 Q1 0.86785324
10 10 Q1 0.02611436
# ... with 20 more rows

```

Or if you don’t want the ID column (not doing anything useful in this particular, made up, case):

```
<pre class="brush: r; title: ; notranslate" title="">

testData %>%
  select(Q1 : Q3) %>%
  gather(key = Question, value = Score, Q1 : Q3)

# A tibble: 30 x 2
  Question Score
  <chr> <dbl>
 1 Q1 0.26001265
 2 Q1 0.34674771
 3 Q1 0.43080742
 4 Q1 0.28397929
 5 Q1 0.14545496
 6 Q1 0.63496928
 7 Q1 0.78777785
 8 Q1 0.44622476
 9 Q1 0.86785324
10 Q1 0.02611436
# ... with 20 more rows

```

Note that by default it will include ALL variables anyway, so this is totally equivalent to:

```
<pre class="brush: plain; title: ; notranslate" title="">

testData %>%
  select(Q1 : Q3) %>%
  gather(key = Question, value = Score)

```

That’s it! As I said at the beginning of the post, I have no idea why I have such a ridiculous mental block about it, it’s all in the documentation, I just get all the columns references and the – notation and all that stuff mixed up (I think partly because using -ID KEEPS the ID variable, it just doesn’t GATHER it). It’s my fault for being an idiot, but the next time I get stuck I’ll read this and understand clearly 🙂

Oh yes, last thing, Q1 : Q3 is just “from Q1 to Q3”, meaing Q1, Q2, and Q3, and Q3 : Q5 would be Q3, Q4, Q5 etc. There are lots of ways to select the variables. See more at ?gather and ?select (which uses the same variable name rules).

One neat trick is num\_range(), which is a shortcut to selecting ranges of things like Q1, Q2, Q3, X1, X2, X3 and so on. You just give the prefix and the numbers you want-

```
<pre class="brush: r; title: ; notranslate" title="">

testData %>%
  select(num_range("Q", 1:3)) %>%
  gather(key = Question, value = Score)

```

Right, I’ll stop now, this post is getting too long.