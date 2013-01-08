---
layout: post
title: "最有用的Linux命令列表"
date: 2013-01-08 14:45
comments: true
categories: ['linux', 'shell']
---

###$ sudo !!###
***Run the last command as root***

Useful when you forget to use sudo for a command. "!!" grabs the last run command.

--------------------------------------------------
###$ python -m SimpleHTTPServer###
***Serve current directory tree at http://localhost:8000/***

--------------------------------------------------
###$ :w !sodu tee %###
***Save a file you edited in vim without the needed permissions***

I often forget to sudo before editing a file I don't have write permissions on. When you come to save that file and get the infamous "E212: Can't open file for writing", just issue that vim command in order to save the file without the need to save it to a temp file and then copy it back again.
<!--more-->
--------------------------------------------------
###$ cd -###
***change to the previous working directory***

--------------------------------------------------
###$ ^foo ^bar###
***Runs previous command but replacing***

Really useful for when you have a typo in a previous command. Also, arguments default to empty so if you accidentally run:
```
echo "no typozs"
```
you can correct it with
```
^z
```
------------------------------------
###$ mtr google.com###
***mtr, better than traceroute and ping combined***

mtr combines the functionality of the traceroute and ping programs in a single network diagnostic tool.
As mtr starts, it investigates the network connection between the host mtr runs on and HOSTNAME. by sending packets with purposly low TTLs. It continues to send packets with low TTL, noting the response time of the intervening routers. This allows mtr to print the response percentage and response times of the internet route to HOSTNAME. A sudden increase in packetloss or response time is often an indication of a bad (or simply over?loaded) link.


--------------------------------------------------
###$ ctrl-x e###
***Rapidly invoke an editor to write a long, complex, or tricky command***

Next time you are using your shell, try typing ctrl-x e (that is holding control key press x and then e). The shell will take what you've written on the command line thus far and paste it into the editor specified by $EDITOR. Then you can edit at leisure using all the powerful macros and commands of vi, emacs, nano, or whatever.

--------------------------------------------------
###$ \<space\>command###
***Execute a command without saving it in the history***

Prepending one or more spaces to your command won't be saved in history.
Useful for pr0n or passwords on the commandline.
Tested on BASH.

--------------------------------------------------
###$ file.txt###
***Empty a file***

For when you want to flush all content from a file without removing it (hat-tip to Marc Kilgus).

--------------------------------------------------
###$ $ssh-copy-id user@host###
***Copy ssh keys to user@host to enable password-less ssh logins.***

--------------------------------------------------
###$ reset###
***Salvage a borked terminal***

If you bork your terminal by sending binary data to STDOUT or similar, you can get your terminal back using this command rather than killing and restarting the session. Note that you often won't be able to see the characters as you type them.

--------------------------------------------------
###$ ffmpeg -f X11grab -s wxga -r 25 -i :0.0 -sameq /tmp/out.mpg###
***Capture video of a linux desktop***

--------------------------------------------------
###$ 'ALT+.' or '\<ESC\> .'###
***Place the argument of the most recent command on the shell***

When typing out long arguments, such as:
```
cp file.txt /var/www/wp-content/uploads/2009/03/
```
You can put that argument on your command line by holding down the ALT key and pressing the period '.' or by pressing <ESC> then the period '.'. For example:
```
cd 'ALT+.'
```
would put '/var/www/wp-content/uploads/2009/03/ as my argument. Keeping pressing 'ALT+.' to cycle through arguments of your commands starting from most recent to oldest. This can save a ton of typing.

--------------------------------------------------
###$ mount | column -t###
***currently mounted filesystems in nice layout***

Particularly useful if you're mounting different drives, using the following command will allow you to see all the filesystems currently mounted on your computer and their respective specs with the added benefit of nice formatting.


--------------------------------------------------
###$ ssh -N -L2001:localhost:80 somemachine###
***start a tunnel from some machine's port 80 to your local post 2001***

now you can acces the website by going to http://localhost:2001/

--------------------------------------------------
###$ echo "ls -l" | at midnight###
***Execute a command at a given time***

This is an alternative to cron which allows a one-off task to be scheduled for a certain time.

--------------------------------------------------
###$ dig +short text <keyword>.wp.dg.cx###
***Query Wikipedia via console over DNS***

Query Wikipedia by issuing a DNS query for a TXT record. The TXT record will also include a short URL to the complete corresponding Wikipedia entry.You can also write a little shell script like:
```bash
$ cat wikisole.sh
#!/bin/sh
dig +short txt ${1}.wp.dg.cx
```
and run it like
```
$ ./wikisole.sh unix
```
were your first option ($1) will be used as search term.

--------------------------------------------------
###$ netstat -tlnp###
***Lists all listening ports together with the PID of the associated process***

The PID will only be printed if you're holding a root equivalent ID.

--------------------------------------------------
###$ dd if=/dev/dsp | ssh -c arcfour -C username@host dd of=/dev/dsp###
***output your microphone to a remote computer's speaker***

This will output the sound from your microphone port to the ssh target computer's speaker port. The sound quality is very bad, so you will hear a lot of hissing.

--------------------------------------------------
###$ curl -u user:pass -d status="Tweeting from the shell" http://twitter.com/statuses/update.xml###
***Update twitter via curl***

--------------------------------------------------
###$ !!:gs/foo/bar###
***Runs previous command replacing foo by bar every time that foo appears***

Very useful for rerunning a long command changing some arguments globally.
As opposed to ^foo ^bar, which only replaces the first occurrence of foo, this one changes every occurrence.

--------------------------------------------------
###$ mount -t tmpfs tmps /mnt -o size=1024m###
***Mount a temporary ram partition***

Makes a partition in ram which is useful if you need a temporary working space as read/write access is fast.

Be aware that anything saved in this partition will be gone after your computer is turned off.

--------------------------------------------------
###$ man ascii###
***Quick access to the ascii table.***

--------------------------------------------------
###$ sshfs name@server:/path/to/folder /path/to/mound/point###
***Mount folder/filesystem through SSH***

Install SSHFS from http://fuse.sourceforge.net/sshfs.html

Will allow you to mount a folder security over a network.

--------------------------------------------------
###$ curl ifconfig.me###
***Get your external IP address***

curl ifconfig.me/ip -> IP Adress

curl ifconfig.me/host -> Remote Host

curl ifconfig.me/ua ->User Agent

curl ifconfig.me/port -> Port

curl ifconfig.me/all -> All

thanks to [ifconfig.me](http://ifconfig.me/)
