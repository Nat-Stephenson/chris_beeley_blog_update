---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-04-03T18:14:49Z"
guid: http://chrisbeeley.net/?p=1344
id: 1344
spay_email:
- ""
title: app.R and global.R
url: /?p=1344
---

I’m doing some Shiny training this year and I want to teach whatever the new thinking is so I’ve been reading Hadley Wickham’s online book [Mastering Shiny](https://mastering-shiny.org/). There’s a couple of things that I’ve noticed where Shiny is moving on, so if you want to keep up to date I suggest you have a look. I’m going to pick out a few here. Firstly, note that in Shiny 1.5 (which is not released at the time of writing) all code in the R/ directory will be sourced automatically. This is a very good idea, I’ve got loads of source(“useful\_code.R”, local = TRUE) lines in some of my applications, so it gets rid of all that.

Something else that’s different is modules are working slightly differently, which I haven’t bothered to absorb yet, I’ve enough on my plate with {golem} but if you’re using modules I suggest to keep abreast and have a look at the [section on modules](https://mastering-shiny.org/scaling-modules.html).

The thing that has just pricked up [various ears on Twitter](https://twitter.com/ChrisBeeley/status/1245729113597530117), though, is the lack of an option on the RStudio option to create separate server.R and ui.R files when creating a new Shiny application through the wizard. I was very surprised when I noticed this. And indeed if you look at Hadley’s book there is no mention of server.R and ui.R, it’s all just app.R. He suggests that if you are building a large application you hive off the logic (in Shiny 1.5, in the R/ folder, or for now by calling source(“file.R, local = TRUE).

But then straight away you’re wondering about where that leaves global.R. For the uninitiated, global.R is a separate file that’s really useful when you’re deploying to a server because it is run only once when the application is run the first time and then will serve its contents to any applications. Its contents is also available to ui.R, which can be helpful setting up the UI based on what’s in the data.

As I mentioned I want to teach where the world is going so I’m trying to do things the new way (I have never taught app.R because, honestly, I hate it, but I guess with the new source R/ folder I can see the reasoning, and I’m not going to argue with the folks down at RStudio anyway 🙂 )

I wasn’t sure about where app.R fitted in with global.R and running on a server so I have written a test application. [You can see the code here](https://gist.github.com/ChrisBeeley/cfddc35235f6119c326d7f70e8ad9120), I’m sorry it won’t run because you don’t have the data and being honest I can’t be bothered to deploy it properly somewhere where you can get it but you get the idea.

The first time the code runs the Sys.sleep(10) runs and you get a big pause. But, sure enough, when you go back it doesn’t run and you get straight in. You can see also that the contents of the datafile are available to the UI (choices = unique(ae\_attendances$Name). Lastly, take my word for it that if you add a file in to the folder called “restart.txt” then it will rerun (and generate the 10 second pause) the next time you go to the application, [just as I used to do with global.R](https://docs.rstudio.com/shiny-server/#restarting-an-application).

That’s all I know at the moment. I hope this is clear and useful (and correct!), it’s all based on stuff I have cobbled together today looking at Hadley’s book and messing around.