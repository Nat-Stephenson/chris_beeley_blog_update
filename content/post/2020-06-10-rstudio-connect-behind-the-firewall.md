---
author: chrisbeeley
categories:
- Uncategorized
date: "2020-06-10T16:18:43Z"
guid: http://chrisbeeley.net/?p=1388
id: 1388
title: RStudio Connect behind the firewall
url: /?p=1388
---

This is part II of what would otherwise have been a far-too-long post about configuring RStudio Connect. A bit of back story, particularly for those of you who might have hit this from a Google search (which does happen, JetPack tells me) and don’t know who the heck I am and what I do all day. Here’s what I said in part I:

> I’ve been using RStudio stuff on the server for a long time. I started using Shiny community edition back in 2013 for an application that is totally open and so doesn’t need authenticating. Then two years ago I started deploying Shiny applications that people authenticated to behind our Trust firewall using Shiny Pro. I have wanted to use RStudio Connect for a long time but it was hard to get the funding together for it given how things are with austerity since the banking crisis.
> 
> \[also, I say later on in the first post- I am NOT DevOps. I’m just a random data scientist trying to get on with his job. So if this is all hilariously wrong/ dangerous/ time consuming, don’t say you weren’t warned\].

<figure class="wp-block-embed-wordpress wp-block-embed is-type-wp-embed is-provider-chrisbeeley"><div class="wp-block-embed__wrapper">> [RStudio Connect in the cloud](https://chrisbeeley.net/?p=1380)

<iframe class="wp-embedded-content" data-secret="KFp9n00b0F" frameborder="0" height="338" marginheight="0" marginwidth="0" sandbox="allow-scripts" scrolling="no" security="restricted" src="https://chrisbeeley.net/?p=1380&embed=true#?secret=KFp9n00b0F" style="position: absolute; clip: rect(1px, 1px, 1px, 1px);" title="“RStudio Connect in the cloud” — chrisbeeley" width="600"></iframe></div></figure>I work for the NHS in the UK and I have an installation of Shiny Pro behind our firewall. It’s running Ubuntu 20.04 on a VM hosted in a Microsoft environment. It authenticates against Active Directory using the LDAP feature of Shiny Pro and uses HTTPS and LDAPS (of course). Authentication to the MS SQL server is done with Kerberos. A cronjob runs overnight pulling data from the data warehouse ready to be loaded into Shiny applications the next day.

The script is simple. Move from Shiny Pro to Connect. The first bit was easy. The LDAP/ AD configuration file looks a little different, but the nice man from IT and I got it working on the second try. HTTPS, LDAPS, also just pretty much cut and paste. So far so good. I haven’t configured the email bit yet, partly because we don’t really need it and partly because I’m not really sure if my Trust will see a Linux server under my control firing off emails on a schedule might pose an IG problem. They would all be routed through the mail servers of my Trust, so it’s no different to me just sitting and sending out emails, but you know what they say, to err is human but if you really want to mess things up use a computer. You don’t need to set up email as long as your authentication is all sorted and the authentication works beautifully.

When you publish something you can very easily add users and groups to the people who can view it, and it even does autocomplete. So for example all the relevant groups for the main suite of applications start with “RS\_T”. So if you type “RS\_T” into the “who can view” box it automatically shows you all the groups that you can add. And you can add people from the staff directory who have never logged in, so you can just add them to everything, send them a link, and they’re off, all using their network password. Beautiful. And as I said in my previous post, that isn’t just ME doing that, it’s any publisher. So the other data scientists can just publish stuff, and add people and groups, and use their exact version of R and their exact version of packages. Compare that with me Filezilla’ing application folders onto a Linux server, testing them, finding they don’t work, and then emailing the person who wrote them saying “it doesn’t work, any idea why?” and them saying “oh I’m using such and such version of tidyr” and me saying “oh I’m not running that on the server, hang on” and back and forwards and… you get the idea.

So that’s what you gain. You can hand off all that responsibility to other people and just do your own thing. But that does come at a price (I’m not saying the price isn’t worth paying. I’m not saying it IS worth paying. I’m just helping you get your head around the migration). There are two things that I got really used to doing with a Shiny Pro installation that you cannot do on a Connect installation that can give you a headache. It’s just my perspective, obviously, if I’d never used Shiny Pro I wouldn’t have this perspective, but I think it can help you understand how it works.

The first most obvious thing that you will miss (and also not miss and think “good riddance”) is the file system. As far as I can tell you have NO access to the file system. You can’t pop .Rdata objects in /srv/shiny-server/applications/data\_store and then load them from several Shiny applications. You can deploy them straight from your hard drive via the RStudio IDE to the server, for example if you’re writing a quarterly RMarkdown report and you have the data sitting on your computer. Or you can use the pins package (<https://github.com/rstudio/pins>). The pins package allows anybody with publishing rights to create a data object on Connect that they and other publishers can point to with their own documents/ applications. Again, it’s a nice way of allowing people to deploy stuff (data in this case) without touching the terminal or using Filezilla or whatever. The related thing that you do not have is cron. Well, you do have cron but without a file system you can’t do anything with it (🙃). So for my use case, where I have a cronjob that processes data and places it somewhere in the file system the conversion for me is to have a scheduled RMarkdown report that does that processing and then uses the pin package to place it on the server where everyone with the right access level can see it. This is nicer, really, because it democratises the sharing of data, otherwise it all has to be placed manually onto a Linux file system. In my case I’m using Kerberos, which means that the top of the script has to get a Kerberos ticket:

```
<pre class="wp-block-preformatted">system("kinit USERNAME -k -y KEYTAB.FILE")
```

Which is totally fine, it just feels really weird and it really didn’t occur to me to do it until I asked somebody at RStudio. But note that the KEYTAB.FILE can’t access the file system either- so you have to deploy it with your application otherwise it won’t be found. You can’t just pop it somewhere safe on the server and forget about it. As I say there’s nothing wrong with this it just feels like putting my jacket sleeves on in the opposite order to how I normally do- fine but weird.

The second thing that you will miss (and, again, think “good riddance”) is the ability to launch R in the terminal on the server, load your packages, run some code, see that it works, shut down the terminal, publish the same code via Connect and have it work every time. The specific set of packages that you have installed on your server, whether you’re running it as the root user or as yourself, bear no relation at all to the packages that will run on the Connect server. Which sounds totally fine but I got myself in a position where the server could run the code quite happily via the terminal, and so could my computer, but it failed when I published it to Connect. This was because the package version that was running on my computer, even though it worked on my computer, wouldn’t install on the server, and although I could install a different, older version of the package, that didn’t help, because Connect doesn’t care what’s on the server, it wants the version that’s on your computer. There is actually a way round this which RStudio encourage you to avoid where possible, and that is to define some packages as using the server version of the package. You can read about this here <https://docs.rstudio.com/connect/admin/r/package-management/>. It looks like this in the config:

```
<pre class="wp-block-preformatted">; /etc/rstudio-connect/rstudio-connect.gcfg
[Packages]
External = ROracle
External = RJava
```

Just be warned that even though they caution against it I had to use it to resolve some conflicts which I think were caused by different versions being available and current on Windows (where the IDE is) and Linux (on which Connect runs).

Phew! That was a long post. I think that’s all I know about that. Feel free to find me on Twitter or email or (in happier times) at a conference and have a chat about it if you’re interested.