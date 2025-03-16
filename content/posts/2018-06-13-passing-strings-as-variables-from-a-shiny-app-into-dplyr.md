---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-06-13T12:31:04Z"
title: Passing strings as variables from a Shiny app into dplyr
---

I’ve been sort of waiting until I understood this thoroughly, and I was going to write a very detailed blog post about it, and although I do understand it a lot better now than I did, I’m still not at the point where I would write an authoritative blog post about it confidently because there are too many things that I don’t understand.

However, I’m aware that there are people out there right now who are trying to write dplyr code that takes strings as variable inputs, passed in from a Shiny interface, and I know how difficult it is to Google, because I Googled it myself. So I’m going to at least show you what to do, and talk a bit about it, and then maybe I’ll come back to it when I’ve understood it better myself.

So you quite often have a combo box in Shiny applications that returns a variable name, and you want to ask dplyr to filter/ select/ whatever on that name. So you have the variable name in the form of a string, and you want to pass it into dplyr. The old way, which still works, but is now deprecated, uses the filter\_ function which evaluates a whole string as if it were dplyr code. So, for example, you might do this:

```
<pre class="brush: r; title: ; notranslate" title="">
library(tidyverse)

input = data.frame("variable" = "cty")

mpg %>%
  filter_(paste(input$variable, " > 18"))

input = data.frame("variable" = "hwy")

mpg %>%
  filter_(paste(input$variable, " > 18"))

```

I’ve given the dataframe the name “input” to make it look like a Shiny application. So imagine your user clicks on “cty” in your app, which makes input$variable equal to “cty”. Now you just paste that together with a filter condition (“&gt; 18”) and pass the whole thing to filter\_(). Now your user perhaps sets the variable to “hwy”, and the calculation can be done with the new value.

So what’s the new way? Well, as I mentioned I don’t understand it deeply, so read the following at your own risk, but essentially what you’re doing is described in this blog post about [dplyr programming](https://dplyr.tidyverse.org/articles/programming.html). You need to quote the variable names which sort of makes them variable-y (told you I didn’t really understand) and then you need to unquote them. Normally you would be doing this:

```
<pre class="brush: r; title: ; notranslate" title="">

input = quo(cty)

mpg %>%
  filter(!!(input) > 18)

input = quo(hwy)

mpg %>%
  filter(!!(input) > 18)

```

So (I think!) you’re saying to R- hey, listen, R, input is cty as a variable name. Wherever you see input, pretend it’s cty but as a variable name. This is called quoting. Then you have to unquote, using !!(). So you’re saying- that thing I mentioned about input being cty quoted? Well, I want it now, unquote input (using !!()) and then use that variable name in the following.

So you’re basically doing that except! Except you’re quoting a string, not a bare variable name. To quote strings, use sym. Other than that, it’s the same. So do this:

```
<pre class="brush: r; title: ; notranslate" title="">

input = sym("cty")

mpg %>%
  filter(!!(input) > 18)

input = sym("hwy")

mpg %>%
  filter(!!(input) > 18)

```

So now you can pass variable names from comboboxes in Shiny applications using dplyr.

I’m sorry I don’t have the detail as nailed as I would like, I wouldn’t advice sitting any exams after reading this, but it should get you coding, at least, which is the first step. If the fog ever truly clears for me I promise I’ll come back and write more.