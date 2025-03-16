---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-03-22T09:48:39Z"
guid: https://chrisbeeley.net/?p=1077
id: 1077
title: Securing Shiny Server over SSL with Apache on Ubuntu
url: /?p=1077
---

Well, I couldn’t find a guide for precisely this on the Internet, securing your Shiny Server over SSL (with Apache), but I did find the following links which were very useful and which I only adapted slightly.

Do have a read of the following if you’re so inclined, they’re both very useful, but neither told me exactly what to do.

<https://ipub.com/shiny-https/>  
<https://support.rstudio.com/hc/en-us/articles/213733868-Running-Shiny-Server-with-a-Proxy>

This guide is for Ubuntu but I imagine the configuration will work on any Apache server. You’ll just need to figure out what the equivalent terminal commands are for whatever OS you’re using.

Even though the changes I’ve made to the config files were quite small I really knew nothing about Apache configuration at all so it’s been a bit of a trial. I’ll try to give a bit of explanation in case it helps you to know what you’re actually doing and show the changes to the config files to make it all work.

I will start with a disclaimer that I am just a dumb guy on the internet. I make no claims as to exactly how secure this is, whether it’s possible to bypass it, any of that. If you’re really interested in security and you don’t know what you’re doing I suggest you pay a security expert to help you. I am many things but a security expert is not one of them. I suppose I should say also that this might not even be the “correct” way to configure Apache. There are lots of different rules of thumb and general guidelines as to what goes where (it still works the other way, it’s just a way of organising things and having a generalisable approach) and I’ve ignored some of them to make this simpler. I’m not a sysadmin so I’ve rather gone with what works rather than what the industry standard is. Again, if that bothers you you’d better pay someone cleverer than me to tell you.

With all that said, let’s begin. The principle of what you’re doing is pretty simple. When you set up your Shiny Server you set it listening on port 3838, by default, or some other port. So now, when people go to, say, chrisbeeley.net:3838 Shiny hears them and delivers whatever it is they ask for. In the meantime Apache is listening on port 80. Every time someone points a web browser at chrisbeeley.net it hollers on port 80 and Apache hears it, and returns a web page. You don’t want that. Shiny isn’t secure over port 3838. What you want is a secure connection with Apache, that then forwards a request on to Shiny.

What we’re going to do is very simple. We’re going to have Apache continue to listen on port 80. However, when someone does go to port 80 Apache will send them to 443, which is secure. Apache already serves my blog with WordPress on chrisbeeley.net/ and my website (which needs an update, I know \*embarrassed emoji\*) on chrisbeeley.net/website. We’re going to have Apache proxy for Shiny Server whenever anyone goes to chrisbeeley.net/shinyapps.

Whenever someone goes to /shinyapps, Apache will say to them “Oh, you want the other guy, sure”. Apache then shouts over to Shiny Server (on port 3838/shinyapps) “Hey! Shiny Server! This guy wants some graphs and whatnot!”. And Shiny Server hears, because they’re listening on 3838/shinyapps, and shouts back “Sure! Here you go!”. And back and forth they go as you click around the application.

We’re going to listen on port 3838/shinyapps, rather than just on plain 3838, because if you don’t, the index page of apps you can optionally display when someone navigates to plain old 3838, instead of 3838/your-app-here doesn’t work properly- all the links to applications are 3838/the-application rather than 3838/shinyapps/the-application. For all I know there’s a better way of dealing with this problem, but this works fine and, as I already mentioned, I’m no sysadmin.

The big difference here from one of the blog posts that helped me to do this is that in their case the whole Apache server was just proxying for Shiny Server. They weren’t listening on port 80 for HTML, because they weren’t serving HTML with Apache. Apache listened on all the ports. I use my server as a web server, so I can’t do that.

So, to summarise, we’re going to have Apache listen on port 80. We’re going to redirect all requests to port 80 to port 443 (HTTPS). And we’re going to have Apache \*proxy for (i.e. shout over to) the Shiny Server on this port. We’ll close the 3838 port on the firewall. Shiny Server will still listen on this port, but nobody outside the server can get to it. Only Apache can, and they will shout instructions to Shiny Server on this port, receiving graphs and buttons back which they’ll show to the user.

The first job is get Apache running. On Ubuntu you’re looking at:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install apache2
sudo apt-get install -y build-essential libxml2-dev

```

Then run a2enmod

This creates a dialog, to which you respond:

ssl proxy proxy\_ajp proxy\_http rewrite deflate headers proxy\_balancer proxy\_connect proxy\_html

That’s Apache taken care of, and the proxying modules set up.

Now you’ve done that the next job is to get an SSL certificate for your site. This used to be difficult and/ or cost money but now it is/ does neither thanks to the wonderful folks at [let’s encrypt](https://letsencrypt.org/). This site is incredibly easy to use, I won’t bother telling you what to do, just shell into your server and follow the instructions. You will be asked if you wish to direct all HTTP to HTTPS. Given that Google Chrome is going to start giving warning messages for \*all HTTP sites, not just ones with passwords/ credit cards, now is a good time to encrypt all your traffic (<https://developers.google.com/web/updates/2016/10/avoid-not-secure-warn>). Let’s do that, the rest of this guide assumes that you do.

If you used to have a Shiny Server that listened on port 3838 and so had that port open, you can now close it. Unless you only want to give your users the option to use HTTPS, and leave the HTTP there for those who want it. You can do that if you want, leave the port open. I won’t go into the firewall stuff here because there are so many ways of configuring firewalls and if you press the wrong button you’ll end up blocking ssh into your server and I don’t want to be responsible for that.

Now for the real magic. We’re going to set up a Virtual host with Apache, on port 443, but only when someone goes to chrisbeeley.net/shinyapps (which, if they use HTTP, will be automagically redirected to HTTPS), which will then act as a proxy server for Shiny Server.

Before we start, it’s worth saying that you may wish to back up your config files. This is very easy. So the next file is /etc/apache2/sites-enabled/000-default-le-ssl.conf. To back it up, just:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo cp /etc/apache2/sites-enabled/000-default-le-ssl.conf /etc/apache2/sites-enabled/000-default-le-ssl.conf_BACKUP

```

That way, if you mess everything up, just:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo mv /etc/apache2/sites-enabled/000-default-le-ssl.conf_BACKUP /etc/apache2/sites-enabled/000-default-le-ssl.conf

```

And everything is back the way it started. Phew!

Right, on to business.

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo nano /etc/apache2/sites-enabled/000-default-le-ssl.conf

```

This will bring up your host definition on port 443

There’ll already be loads of stuff at the top about port 443. Leave that alone. It knows what it’s doing. We’re going to add some stuff at the bottom.

```
<pre class="brush: bash; title: ; notranslate" title="">
Stuff at the top...

ProxyPreserveHost On
ProxyPass /shinyapps http://0.0.0.0:3838/shinyapps
ProxyPassReverse /shinyapps http://0.0.0.0:3838/shinyapps
ServerName localhost

</VirtualHost>
```

As you can see, we route everything for /shinyapps to port 3838/shinyapps.

You’ve done! All HTTP traffic is now routed to HTTPS. If it’s /shinyapps, it goes to Shiny Server. If not, it goes to Apache for a normal web request.

Well done, have a cup of tea to celebrate.