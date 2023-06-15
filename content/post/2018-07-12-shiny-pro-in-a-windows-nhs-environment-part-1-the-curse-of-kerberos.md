---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-07-12T14:02:53Z"
guid: https://chrisbeeley.net/?p=1124
id: 1124
title: 'Shiny Pro in a Windows/ NHS environment, part 1: The curse of Kerberos'
url: /?p=1124
---

Right, here goes. Let’s tell the story. I’ve been working on this for months, not because it’s particularly hard, but because I really didn’t know what I was doing. We are a health trust in the UK (NHS), and we’ve been using Shiny on a privately hosted Linode server (Ubuntu) in the cloud for a long time. You can see it [here](http://109.74.194.173:8080/apps/SUCE/). We’ve done this (hosted it in the wild) because none of the data is private. It’s patient experience data. We want it to be public. If anything, we want people to steal it. To be honest it’s looking a bit tatty. I was proud of it in 2012, now not so much. I’m working on a new version.

Anyway, we now want to use Shiny with actual patient data. This means two things. Firstly, we have to host the data behind our firewall. And secondly, we need to authenticate users. We don’t want just anybody in the Trust getting access to it. I should say that in truth, the information governance requirements are fairly mild, since all the data is aggregated and/ or pseudonymised. There is no personal data in the application anywhere. Anyway, we want to authenticate users.

So we went out and bought Shiny Server Pro. As a keen user of R and Shiny, I’m very excited about this. I think it very likely that we are the first Trust in the country to buy a Pro licence. There’s some debate in the Trust about R’s place in our BI ecosystem, which I won’t go into here, but nonetheless, we’re doing it, now, and it’s very exciting for me.

So it was my job to make it all work. This means two things. Firstly, we need the server to authenticate against MS SQL Server so we can fetch the data nightly from a data warehouse. If you use passwords, obviously, it’s easy, but my Trust likes to be more secure than that so Kerberos it is. And secondly we need to use the LDAP features of Pro to authenticate users against the Active Directory user database which the Trust maintains. This post will just be about Kerberos. I’ll write up the LDAP stuff another day (and add a link here once I do). The actual server, in case you’re wondering, is Ubuntu 16.04 running as a virtual machine on the Trust’s completely Windows setup.

Now, if you’re as ignorant as I was at the start it will help you to understand what Kerberos is. Basically, it’s a little program that lives on the Trust’s servers and it issues tickets if you’re allowed to do something. So all you need to do is install a client program which goes to Kerberos and says “Hello, I’m so and so, and my password is such and such, can I have a ticket please?” and Kerberos looks it up and if the server’s name is, as they say, “down”, it gives the server a ticket. This ticket lasts for a certain period of time (I think the Kerberos server gets to configure this) and it will let the client server do all the stuff on the ticket. So I need a ticket that says “let this guy into this bit of the data warehouse”, and Kerberos gives me a ticket, and then I can go to SQL Server, show the ticket, and be let in.

I’m going to do my best to explain how to configure it all. I’m no expert (obviously!) so I will probably miss bits out but I personally would have found a blog post like this that gave a practical sequence of what to do useful when I was learning, so I hope you will find it useful. And if you don’t, as usual it will help me in 6 months when I forget it all :-).

First things first, note that the clocks on the client and server must match. Mine did, so I didn’t worry about this. Right, let’s look first at where we’re trying to get to. Right at the top of my R code I want to run this:

```
<pre class="brush: bash; title: ; notranslate" title="">
odbcDriverConnect(connection =
  "Driver={SQLServer};
  server=serverName;
  database=dbName;
  trusted_connection=yes;")
```

This will run fine if you have the right Kerberos ticket, and not at all if you don’t. Next let’s make sure we’ve got some ODBC drivers. As a user of Shiny Server Pro, I get access to RStudio’s paid drivers, so I used them. Microsoft have some for download, too. I also get access to RStudio support, which I have to say is excellent. Install them. You’ll need to configure the name and file path in here: /etc/odbcinst.ini (the name of the driver in the odbcDriverConnect above is defined in this file).

Okay. So far so good. Now we’re getting to the more tricky parts. Next you’ll need a Kerberos client:

```
<pre class="brush: bash; title: ; notranslate" title="">
sudo apt-get install krb5-user
```

Ask your IT department about the location of their Kerberos server. You’ll need to put it in the \[realms\] section of /etc/krb5.conf, like so:

\[realms\]

EXAMPLE.ORG = {  
 kdc = example.org:88  
}

Obviously replace example.org with whatever your IT department tell you. Note you put it in twice, the first time in capital letters. I’m not going to insult your intelligence by pretending I understand why that is.

And also here:

\[libdefaults\]

default\_realm = EXAMPLE.ORG

Okay, getting there now. You’ll need a username and password from IT. To test Kerberos just run

```
<pre class="brush: bash; title: ; notranslate" title="">
kinit -p username@EXAMPLE.ORG
```

Again, the server name is in capitals. Again, I do not know why. You’ll be prompted for your password. Hopefully that should work. If it does, you now have a ticket. You can test your connection to SQL Server using isql ([https://docs.oracle.com/cd/E88353\_01/html/E37839/isql-1.html](https://docs.oracle.com/cd/E88353_01/html/E37839/isql-1.html)).

Wait, you’re still not done! I’m assuming, like me, you want to run the whole thing in a script, either because you want to run overnight as a cron job (as I did) or just because you don’t want to faff around with terminal commands every time you run an R script. Your credentials will expire, so we need to be able to get them again in the script. You can save your password in a script BUT DON’T. It’s bad practice, and I feel sure that your IT department will get cross. You need a keyfile. I’m not going to go through all of that, there’s a fair bit to it, but there is an excellent [guide to using a keyfile here](https://kb.iu.edu/d/aumh), go and follow it.

That’s it! You’ve done it! Now all you need to do is run the following at the terminal, replacing the /home/chrisbeeley bit with the name of your keyfile, and the scriptName.R with your script.

Before I go, there’s quite a useful page here, which goes into a bit more detail than I have [https://www.easysoft.com/products/data\_access/odbc-sql-server-driver/kerberos.html](https://www.easysoft.com/products/data_access/odbc-sql-server-driver/kerberos.html).

This took me WEEKS, because I DID NOT KNOW WHAT I WAS DOING. It’s my sincere wish that it helps you if you want to do it. I’m more than happy to talk it through with anyone out there, as I mentioned I’m not an expert but I might have had a similar problem to you. Find me @ChrisBeeley on Twitter. And that goes double for anybody who works in the NHS. It’s one of my career goals to support the adoption of Linux/ R/ Shiny in the NHS and if I can help at all, I will.

Tune in next time for the LDAP story, which was even more hideously difficult (at least, for my tiny brain).