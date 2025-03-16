---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-11-06T18:03:08Z"
title: Authenticating using LDAP from Active Directory using Shiny Server Pro
---

Right, I promised I would write this a very long time ago and I still haven’t done it. I’ve just got back from the NHS-R community conference (see an upcoming post) and I’m in a sharing mood, and besides someone just asked me about this on Twitter.

So setting up LDAP based authentication on Shiny Server Pro via Windows Active Directory. Do note that this post is about using Active Directory. If you’re not doing that a lot of this stuff will not apply.

I made the most most horrible mess of this when I tried, because quite frankly I didn’t have the slightest idea what I was doing. I’m sure your setup will be different from mine, but I can offer some general advice. Do read the documentation as well, obviously, this is only going to fill in the blanks, it’s not going to replace the [docs](https://docs.rstudio.com/shiny-server/).

The first piece of advice I can give you is don’t try to get it working all at once in the configuration file. Unless you’re some sort of Linux security genius it’s not going to work. Instead use the [ldapsearch](https://linux.die.net/man/1/ldapsearch) command to make sure that you’ve got the right server and your search string is correct. You’re going to need the help of IT who can tell you a bit about what the search string might look like. To give you a clue, here’s what I finally got working:

ldapsearch -H ldap://xxx.xxx.nhs.uk/ -x -D “CN=User.name,OU=Group,OU=Group2,OU=Group3,DC=xxxx,DC=xxx,DC=nhs,DC=uk” -b “OU=Users All, DC=xxx,DC=xxxx,DC=nhs,DC=uk” -w password “(&amp;(cn=test.user))”

You can see the location of the LDAP server at the beginning, I had to take the details out because I was told that publicising the server name poses a security risk. That first CN=User.Name is the LDAP user. You can just use yourself or IT can set you up a special LDAP login (you will probably once to switch to this once you’ve got everything working). Then in that string the next bit is where the user is in terms of their place on the tree and the DC bit is the domain and subdomain split up into bits. Again I’ve redacted it. Imagine it might say something like DC=mytree, DC=myorg, DC = nhs.uk if the LDAP server was on mytree.myorg.nhs.uk.

Then the string after, the bit that says “OU=Users All…” shows where the person you’re searching for in the tree is. You can search for yourself, or someone else. There may be more than one OU group, depending on what IT tell you, and then the DC bit is the same as it was in the previous bit. Then you need -w password (this is yours or your LDAP user’s password) or if you don’t want to put your password into the terminal use

-W which will prompt for your password. And the last bit as you can probably guess is the username of the person that you’re searching for. Once it works it will bring back a big list of all the groups and stuff that the user is registered in.

So now you’ve got that working you can edit /etc/shiny-server/shiny-server.conf to add the authentication stuff. You probably already did something with it setting up application directories etc.   
The thing that I got working was as follows (again with bits redacted, I’m afraid):

auth\_active\_dir ldap://.nhs.uk/DC=xxx,DC=xxxx,DC=nhs,DC=uk xxx.xxxx.nhs.uk{

 base\_bind “CN=user.name,OU=Location,ou=GroupXXX,OU=GroupYYY,{root}” “LDAP\_user\_password”;  
 user\_bind\_template “{username}@xxx”;  
 user\_filter “sAMAccountName={username}”;  
 user\_search\_base “OU=GroupXXX”;  
 group\_search\_base “OU=GroupXXX,OU=GroupYYY”;  
}

Do have a look at the examples in the documentation as well, it will help you to understand what yours might look like. The base\_bind line has the actual user name in the first bit, e.g. CN=chris.beeley or CN = ldap.user, and the actual password at the end. This is not very secure. Your connection is also being made over LDAP, not LDAPS. Also not very secure.

Let’s fix both those problems now. Add an “s” to the server name at the beginning (ldaps://…). Then you’ll need to point to a certificate on the trusted\_ca line. IT should be able to give you one. Note that it’s a ROOT certificate you need. IT gave me loads that were not root certificates which did not work. I ended up downloading it myself from the Intranet through Chrome which messed up the domain name and I never quite resolved it to be honest. It works, that ended up being enough by the end. Note also that the certificate must be in PEM format.

auth\_active\_dir ldaps://.nhs.uk/DC=xxx,DC=xxxx,DC=nhs,DC=uk xxx.xxxx.nhs.uk{

 trusted\_ca /…pathtodir/certificate3.pem;  
 base\_bind “CN=user.name,OU=Location,ou=GroupXXX,OU=GroupYYY,{root}” “/pathtopassword/password.txt”;  
 user\_bind\_template “{username}@xxx”;  
 user\_filter “sAMAccountName={username}”;  
 user\_search\_base “OU=GroupXXX”;  
 group\_search\_base “OU=GroupXXX,OU=GroupYYY”;  
}

You can see also that we’ve written the file path to a text file containing the password on the second line. It’s a good idea to modify the permissions of this so only root can read it, to keep it safe.  
That’s it! You’ve done it! Well done! That took me weeks.

Wait! One more thing. You now have your staff putting their actual usernames and passwords into a web form over HTTP. This is VERY insecure. We need to secure over SSL. This is pretty easy. You just need a key and a certificate for the server from your IT department. Tell Shiny to listen on 443 (instead of, say, 3838) as below and put the key and certificate file path in. So the top of your config will look like this.

server {

 # Instruct this server to listen on port 443, the default port for HTTPS traffic  
 listen 443;

 ssl /etc/shiny-server/KEY.key /etc/shiny-server/certificate.cer;

… etc.

Your users can now connect at https://serveraddress

That’s really it now. Well done. Have a cup of tea to relax and celebrate a job well done. I apologise that this post is a little light on details, it will be very different I think depending on where you are, but if I’d read this post before I’d started it would have helped me, and I hope it helps you too.