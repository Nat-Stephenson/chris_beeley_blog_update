---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-03-23T14:41:14Z"
guid: http://chrisbeeley.net/?p=593
id: 593
title: Setting up a new Linux install, quickstart guide
url: /?p=593
---

This is another post which is designed both to help me as an aide-memoire and also to help anyone else out there who wants to set up a new Linux install quickly and easily.

One of the many, many things I love about Linux is the way that you can just tear down your OS any time you like and start using a different one. Having said that, I must admit to having had some long weekends and evenings when my installation has gone kaput fiddling around trying to get everything working. I have now streamlined the process and can set up a new Ubuntu-based distribution in probably no more than about 45 minutes. My Linux career has only been from Ubuntu to the Ubuntu-based Mint so far, I have tried to use Fedora but sadly my ridiculously powerful NVidia graphics card left over from my (whisper it) Windoze days does not appear to play nicely at all with Fedora. Going to have a go with Fedora 20 some time, you never know your luck.

So with no further ado here is my speedy install process that I run every time I change distros (Mint 15 to 16, or just if I break one, which I do, from time to time, fiddling). I’m no Linux expert, so there may be one or two howlers in here, but it works for me, anyway, so use at your own risk.

This is all from a Linux Mint 16 install, and should work fine for Ubuntu proper, and may well work for other Ubuntu based distributions, I’m afraid I don’t know for sure.

Once the CD is in you will be offered the chance to boot to a Live-CD version.

If, like me, you have a ridiculously powerful NVidia graphics card you will want to press “e” to edit the commands before launch and change the bit that says “Quiet splash” so that it says “nomodeset”. This will launch the OS in a low graphics version.

Next select the icon to Install and reboot.

If you’ve got loads of different Linux and Windows OS’s on your machine as I have you may find that the GRUB loses track of things a bit, in which case just launch the OS that does work and run sudo update-grub, which should put things right again.

Before you reboot into the new install (if you have a crazy graphics card) press “e” and once again change the bit that says “Quiet splash” so that it says “nomodeset”. Then (if you’re using NVidia) run:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install nvidia-current

```

Reboot again without changing to nomodeset and you should find that your graphics works fine, if not I’m afraid it’s off to the forums with you.

Now to bring everything up to date:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get update
sudo apt-get upgrade

```

At this point we now have a fresh install, all up to date and playing nicely with our graphics card. The following list of software obviously reflects my own preferences, so make of it what you will.

Let’s install [Google Chrome](http://www.ubuntuupdates.org/ppa/google_chrome)

This puts the key in:

```
<pre class="brush: bash; title: ; notranslate" title="">

wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add - 

```

Repository setup:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'

sudo apt-get update 
sudo apt-get install google-chrome-stable

```

Now let’s install [The Mighty R](http://craig-russell.co.uk/2012/05/08/install-r-on-ubuntu.html#.UvlAA3V_uhO)

Run the following at the terminal:

```
<pre class="brush: bash; title: ; notranslate" title="">

gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -

```

Edit the sources list:

```
<pre class="brush: bash; title: ; notranslate" title="">

gksudo gedit /etc/apt/sources.list

```

And add the following (saucy is for Mint 16/ Ubuntu 13.10, you will want a different version if you are running an older distro (or a newer one, if you’re reading this in the future):

deb http://cran.rstudio.com/bin/linux/ubuntu saucy/

Back to the terminal:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get update
sudo apt-get install r-base
sudo apt-get install r-base-dev

```

There are a few dependencies that I find are common in R packages (e.g. for RCurl, XML, and anything that uses Java), install them all like this:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install openjdk-7-jdk libcurl4-gnutls-dev libxml2-dev

```

Install [R Studio](http://www.rstudio.com)

Just directly install from [here](http://www.rstudio.com/ide/download/desktop)

Install [Dropbox](https://www.dropbox.com)

Just directly install from [here](https://www.dropbox.com/install)

I’ll be honest and say LaTeX/ Sweave is starting to take a back seat for me, reproducible programming wise, and being replaced with Markdown/ RMarkdown/ Knitr, but I have loads of old Sweave scripts and I haven’t abandoned it completely, so I still need a LaTeX install. One liner:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install texlive-full

```

It’s a big install so go get yourself a cup of tea. The smaller one will not handle all the things that Sweave throws at it, in my experience, so you may as well go in large.

I’m an absolute sucker for programs that rotate your wallpaper (by which I mean, fetch you new ones, not turn it round, that would be annoying). Here’s a wonderful one:

[Variety wallpaper changer](http://peterlevi.com/variety/)

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo add-apt-repository ppa:peterlevi/ppa
sudo apt-get update
sudo apt-get install variety

```

Once you’ve got it set up I recommend the following sources of wallpapers if you’re a massive geek like me:

http://wallpapers.net/games-desktop-wallpapers.html  
http://wallpapers.net/linux-desktop-wallpapers.html  
http://wallpapers.net/nature-desktop-wallpapers.html  
http://wallbase.cc/tags/info/8003 (games)

http://wallbase.cc/tags/info/16530 (Marvel comics)

I edit my HTML/ Java/ other random things in the wonderful [Geany](http://www.geany.org/) IDE which supports compilation of code, preview of HTML and various other useful bits and pieces

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install geany

```

And, for those of us cursed with annoying Citrix email systems, Linux is all over that these days, it’s a bit fiddly but well worth it (in some ways the Linux client actually works better than it does on my Windoze 7 machine).

Not a lot of point my going through all the stuff that’s on [this](https://help.ubuntu.com/community/CitrixICAClientHowTo#Citrix_Receiver_12.1_on_Ubuntu_13.10_64-bit) incredibly useful page.

UPDATE: I found recently that Firefox stopped playing nicely with Citrix, probably due to the big update Firefox has just had, but you can get it working on Chrome, too (which is my preferred browser anyway). Instructions are [here](http://ubuntuforums.org/showthread.php?t=1645173).

The wonderful [Conky](http://conky.sourceforge.net/) which I confess I find rather difficult to use without the [GUI manager](https://launchpad.net/conky-manager) that you can get for it.

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install conky
sudo apt-add-repository -y ppa:teejee2008/ppa
sudo apt-get update
sudo apt-get install conky-manager

```

And last but not least the mighty [Git](http://git-scm.com/download/linux), which, wonderfully, once you’ve got it installed can then be used to fetch its own latest version. A bit of recursion there for all you recursion fans out there.

Another one liner:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo apt-get install git

```

Boom! You’ve got yourself a brand new Linux box, filled with top quality software, for free, in under 45 minutes.