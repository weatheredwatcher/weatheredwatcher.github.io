---
date: "2014-08-23T00:00:00Z"
date_published: "2014-08-23T04:11:28.000Z"
date_updated: "2015-05-19T00:20:34.156Z"
slug: server-stuff-a-developer-needs-to-know
title: Server Stuff a Developer Needs to Know (part 1)
---

Recently I have been moving more and more away from development and more into System Administration.  Still, my skills and experience in programming 

means that I also have a good handle on developers. So I have been making a effort to move in the direction of DevOps.  I do some consultation work for 

a startup that is currently coming out of the USC Incubator.  I mostly contribute as the Architect and help provide direction to the infrastructure and 

coding best practices as well as run their servers.  So this fits perfectly in the role of DevOps.  One of the things that has really begun to frustrate me 

is the lack of BASIC linux skills in developers that should know better.  I am not asking you to setup a fucking mail server or configure a load balancer. 

I can do those things just fine without your help.  But there are certain things that a developer NEEDS to know how to do. So, here is the first in a series of posts 

on some things that in my humble opinion a developer should be able to do without me holding their goddamn hand!

## Logging into and Deploying Code to a Server  

Guys, this is not that hard.  First of all: A secure server should be using ssh -- NOT ftp.  Do not ask me for FTP access to my server.  I will not 

give it to you.  No matter how many times you ask me to. 

You can connect to a server using ssh like this:

    ssh username@hostname
    

If you do not want to use a password, or if I decide not to give you one, you will need to provide me with a public key.  It is fairly easy.  On a 

Linux/Unix system, in the termianl type:

    ssh-keygen -t rsa -f ~/.ssh/id_rsa
    

This will give you a key pair. Email me the one with the .pub extension

    id_rsa.pub
    

Running Windows?  Either install git and use git-bash or install Cygwin and run the Bash shell.  The above commands will work for you too.

Then you should be able to log in without a password.   Having issues?

    ssh user@hostname -i ~/.ssh/id_rsa.pub
    

Rather do it yourself? Once you have logged in to the server with your given user name, you can add your key to the authorized_keys file.

It needs to be in a file called  ~/.ssh/authorized_keys

Now on to deployment.  Most of the time there is going to be some procedure in place for production deployments, but not so much on a dev server. 

You should be able to upload your code to the server. Aside from using git, which is just easy, I personally like secure copy or scp.  It is fairly simple to use, 

especially if you already have a key setup on the remote server.  The syntax is easy:

    scp -r localfolder/ user@remotehost:/path/to/server
    

If you would rather use rsync:

    rsync -r localfolder/ user@remotehost:/path/to/server
    

It's not rocket science people!  Learn some basic Linux!
