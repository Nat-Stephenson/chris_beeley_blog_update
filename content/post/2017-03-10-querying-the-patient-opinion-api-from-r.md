---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-03-10T11:28:06Z"
guid: http://chrisbeeley.net/?p=886
id: 886
title: Querying the Patient Opinion API from R
url: /?p=886
---

[Patient Opinion](https://www.patientopinion.org.uk/) have a new API out. They are retiring the old API, so you’ll need to transfer over to the new one if you are currently using the old version, but in any case there are a number of advantages to doing so. The biggest one for me is the option to receive results in JSON (or JSONP) rather than XML (although you can still have XML if you want). The old XML output was fiddly and annoying to parse so personally I’m glad to see the back of it (you can see the horrible hacky code I wrote to parse the old version [here](http://chrisbeeley.net/?p=487)). The JSON comes out beautifully using the built in functions from the R package [httr](https://cran.r-project.org/web/packages/httr/index.html) which I shall be using to download and process the data.

There is a lot more information in the new API as well, including tags, the URL of the story, responses, status of the story (has response, change planned, etc.). I haven’t even remotely begun to think of all the new exciting things we can do with all this information, but I confess to being pretty excited about it.

With all that said, let’s have a look at how we can get this data into R with the minimum of effort. If you’re not using R, hopefully this guide may be of some use to you to show you the general direction of travel, whichever language you’re using.

This guide should be read in conjunction with the excellent API documentation at Patient Opinion which can be found [here](https://www.patientopinion.org.uk/info/api-v2).

First things first, let’s get access to the API. You’ll need a key which can be obtained [here](https://www.patientopinion.org.uk/mysubscriptions). Get the HTTP one, not the Uri version which only lasts for 24 hours.

We’ll be using the HTTP protocol to access the API, there’s a nice introduction to HTTP linked from the [httr](https://cran.r-project.org/web/packages/httr/vignettes/quickstart.html) package [here](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177). We will be using the aforementioned httr package in order to make requests using the HTTP protocol.

Let’s get started

```
<pre class="brush: r; title: ; notranslate" title="">

# put the subscription key header somewhere (this is not a real API key, of course, insert your own here)

apiKey = "SUBSCRIPTION_KEY qwertyuiop"

# run the GET command using add_headers to add the authentication header

stories = GET("https://www.patientopinion.org.uk/api/v2/opinions?take=100&skip=0",
              add_headers(Authorization = apiKey))

```

This will return the stories in the default JSON format. If you prefer XML, which I strongly advise you don’t, fetch the stories like this:

```
<pre class="brush: r; title: ; notranslate" title="">
stories = GET("https://www.patientopinion.org.uk/api/v2/opinions?take=100&skip=0",
add_headers(Authorization = apiKey, Accept = "text/xml") )

```

For those of you who are using something other than R to access the API, note that the above add\_headers() command is equivalent to adding the following to your HTTP header:

Authorization: SUBSCRIPTION\_KEY qwertyuiop

You can extract the data from the JSON or XML object which is returned very simply by using:

```
<pre class="brush: r; title: ; notranslate" title="">
storiesList = content(stories)

```

This will return the data in a list. There is more on extracting the particular data that you want in another blog post which is parallel to this one [here](http://chrisbeeley.net/?p=904), for brevity we can say here that you can return all the titles by running:

```
<pre class="brush: r; title: ; notranslate" title="">
title = lapply(opinionList, "[[", "title")

```

That’s it. You’re done. You’ve done it. There’s loads more about [what’s available](https://www.patientopinion.org.uk/info/api-v2-content) and [how to search](https://www.patientopinion.org.uk/info/api-v2-filtering) it in the help pages, hopefully this was a useful intro for R users.