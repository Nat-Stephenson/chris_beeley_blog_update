---
author: chrisbeeley
categories:
- Uncategorized
date: "2016-04-01T15:33:30Z"
guid: http://chrisbeeley.net/?p=779
id: 779
title: Backing up a remote MySQL database on a remote server using cron
url: /?p=779
---

This is more a collection of links, really, than a proper blog post, but I just spent quite a while figuring out how all of these different technologies all fit together to do the job I wanted so if you want to do what it says on the tin of this blog post then you should find it useful.

I have a remote server running a MySQL database. I wish to backup this server daily. I would rather do this on my other remote server than on my local machine because, obviously, my server is always switched on whereas my local machine isn’t. So the task is to dump a MySQL database to a file and upload it to a remote server.

To begin, you’re going to make sure that the server with the MySQL database on it can access the other server. This is easiest using ssh key login. I use this for all my servers and all of my local machines. There is a summary [here](https://help.ubuntu.com/community/SSH/OpenSSH/Keys). The beauty of using a key based login is it eliminates the need for passwords, which is not only secure in general but also means you do not need to store passwords in cron scripts or anywhere else on the computer which is highly insecure. Unless you really know what you’re doing you should never do this.

Next you’ll need to create a user on your MySQL and give it SELECT privileges so it can read the data to make the backup. This is very simple

GRANT SELECT, LOCK TABLES ON mydatabase.\* to ‘backup’@’localhost’ IDENTIFIED BY ‘password’;

Now you need to set up a cron job to back up the database every day. There’s some stuff about it [here](http://www.techiecorner.com/1619/how-to-setup-mysqldump-without-password-in-cronjob/), but you won’t need the bit about passwords because we’re using key authentication. As I mentioned above, it’s easier and safer. We’re also going to add some terminal magic to automatically change the file name every day to the current date so you can have daily backups on the server. Here’s the finished cron entry.

0 1 \* \* \* mysqldump -u backup SUCE | ssh chrisbeeley@178.79.158.221 “cat &gt; ~/backupDB/$(date +\\%F)\_SUCE\_DB.sql”

This runs the backup operation every day at 1am.

You’ll need to clean up the old files occasionally, otherwise your server will get full of very old copies of the database. The following command will remove all files from the directory that are older than 31 days old:

find ~/backupDB -type f -mtime +31 -exec rm {} +

BE VERY CAREFUL with this command. If you run it with the wrong directory or have any strange errors with it you may find yourself deleting a lot of files that you did not want to delete. ALWAYS be safe and test by running find ~/backupDB -type f -mtime +31 first and reviewing the files which come back. Only when you’re happy that it’s finding the right files should you add -exec rm{} +

I haven’t added this to a cron job, partly because it doesn’t seem worth the risk (the command is safe in my environment today, but always be wary of [PEBKAC](https://en.wikipedia.org/wiki/User_error), even for yourself), and also because it means I can choose when to flush the old database files, just in case I find some current problem in the database which I can fix with an older version.