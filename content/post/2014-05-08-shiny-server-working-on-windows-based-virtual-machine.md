---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-05-08T21:00:15Z"
guid: http://chrisbeeley.net/?p=624
id: 624
title: Shiny server working on Windows based virtual machine
url: /?p=624
---

Working in the NHS as I do, which runs all of its IT on Windows, I’d always despaired of being able to get [Shiny Server](http://www.rstudio.com/shiny/server/) working. However, I went to see the IT department and they have very kindly given me access to a virtual machine with Ubuntu 12.04.04 LTS running on it. It’s completely firewalled from the internet and is only visible from within the network, which is quite useful because it means I can put up things on it which are not for public consumption.

So the question was, will it run Shiny server without any problems? I’m very happy to say yes. This is a great development since it means I can run Shiny Server behind our corporate firewall and share things with people in the organisation with it very easily. Here’s a quick guide to what I did to get it working. I can’t say for sure if it will all work for you, depends on ports and probably lots of random other things, but it can’t hurt to try.

First job, of course, is to install [R](http://craig-russell.co.uk/2012/05/08/install-r-on-ubuntu.html#.UvlAA3V_uhO). Follow the link for a great summary of installing at the command line.

DON’T, as the Shiny Server documentation suggests, just run apt-get install r-base because the version in the repository is only 2.14 for which a lot of important packages are not available

I found on an NHS network that I had port problems, so instead of running:

```
<pre class="brush: bash; title: ; notranslate" title="">

gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9

```

Run this instead.

```
<pre class="brush: bash; title: ; notranslate" title="">

gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-key E084DAB9

```

Worked for me anyway.

Now run:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get update

```

For me, this generated an error (again because of port problems):

GPG error: \[…\] The following signatures couldn’t be verified because the public key is not available: NO\_PUBKEY 51716619E084DAB9

So I ran this:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 51716619E084DAB9

```

Now install:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install r-base-dev

```

Now let’s install Shiny Server, follow the instructions [here](https://www.rstudio.com/shiny/server/install-opensource).

Boom. Navigate to 127.0.0.1:3838 (put your actual IP in here, obviously) and you should find the example application, give it a test, and you’re off.

Once you start running applications you will no doubt need to install R packages, I find it easier to launch R, run .libPaths() and make myself the owner of one of these directories:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo chown chrisbeeley /usr/lib/R/site-library/

```

That way you can just run install.packages(“ggplot2”) from within R rather than faffing around with R CMD INSTALL and all the packages will install into the site library.

Enjoy!