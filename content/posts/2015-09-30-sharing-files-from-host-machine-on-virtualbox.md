---
author: chrisbeeley
categories:
- Uncategorized
date: "2015-09-30T11:45:28Z"
guid: http://chrisbeeley.net/?p=769
id: 769
title: Sharing files from host machine on virtualbox
url: /?p=769
---

I’ve really started getting into virtual machines recently. Firstly because I like to try different flavours of Linux and constantly reinstalling partitions is starting to get a bit of a drag, especially when it often takes ages to get the graphical desktop to work because of my silly graphics card. Secondly, and more importantly, because I’m finding if you want to do programming, or especially write about programming, you often need a “clean” system or a system set up a particular way to test things and work on multiple things at once. So the code I write for [Shiny](http://shiny.rstudio.com/) is based on the newest 0.12 version. However, my server is all the way back on 0.7 for various reasons to do with browser compatibility. So this means I need a local 0.7 installation to test programs that I’m updating on the server. Or sometimes it can be important to check what the dependencies for your programs are. You may have installed something a year ago that makes everything work beautifully that you’ve long since forgotten about, and if you tell someone else to do what you’re doing it won’t work on their machine. In the Linux world technologies like [Docker](https://www.docker.com/) which package up whole environments at the operating system level and help you to [manage dependencies](https://deis.com/blog/2015/sailing-past-dependency-hell-with-docker) (amongst other things) are starting to spring up and in the R world things like [packrat](https://rstudio.github.io/packrat/) roll up packages into bundles in order that all package versions are the same and dependencies satisfied.

I haven’t learned those technologies yet so I’m going to stick with virtualisation for now (Docker is probably a bit of a sledgehammer to crack a nut for me, anyway, really).

I am mainly making this post because I keep forgetting how to access the host file system on the guest system, so I’ll be able to refer to it in future. I hope it helps you. It’s based on Ubuntu. Some of it is probably similar in other OS’s, I really have no idea I’m afraid.

Go to Devices (on the Virtualbox host machine, i.e. on the window that contains the graphical environment of the guest machine) and select Insert Guest Additions CD image. It will be mounted for you. You need to find out where it’s mounted so you can access it from the terminal. Mine mounted at /media/chris/VBOXADDITIONS\_4.3.10\_93012/.

Run the Linux script within the mounted folder, on my machine it was:

```
<pre class="brush: bash; title: ; notranslate" title="">

cd /media/chris/VBOXADDITIONS_4.3.10_93012/

sudo sh ./VBoxLinuxAdditions.run

```

Presuming this works okay, reboot the virtual machine, and now select Devices… Shared folder settings on the host machine. Click “Add shared folder” over on the right. Find the folder and click “Auto-mount”. Select “Read only” (a lot of people seem to do this, don’t know why, seems safer anyway).

If you restart the folder should now be in /media. If you get “Unknown filesystem”, “Cannot open folder”, or something like that then you need the advice from [here](http://ubuntuforums.org/showthread.php?t=2075432).

From the VBox User Manual:

Access to auto-mounted shared folders is only granted to the user group vboxsf, which is created by the VirtualBox Guest Additions installer. Hence guest users have to be member of that group to have read/write access or to have read-only access in case the folder is not mapped writable.

Therefore run:

```
<pre class="brush: bash; title: ; notranslate" title="">

sudo adduser yourusername vboxsf

```

Restart and it should all work. I restarted the *virtual* machine after each stage, just in case. If it still doesn’t work, sorry, off to the forums with you.