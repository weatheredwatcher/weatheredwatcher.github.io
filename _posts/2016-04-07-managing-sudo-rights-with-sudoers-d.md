---
date: "2016-04-07T00:00:00Z"
date_published: "2016-04-07T19:25:24.145Z"
date_updated: "2016-04-07T19:25:24.144Z"
slug: managing-sudo-rights-with-sudoers-d
title: Managing sudo rights with sudoers.d
---

It's been a while since I have posted to the blog...been busy!  Today I'd like to talk about the good old `sudoers` file.

The `sudoer` file has always been the way to manage the users with sudo privileges on your server.  There is another way to manage the users.  If you look at the base of your sudoers file, will see the lines:
`#includedir /etc/sudoers.d`

A look at the `/etc` folder and we see a folder, `/sudoers.d`.

Place a file in here with sudoers configuration and it will include it along with the `sudoers` file.

I like to put in files for different access groups...like a file called `developers` that I use to give sudo rights to the development team.  

Another thing you could do would be to add a file for *each* developer so that removing a users and all privileges from the server is as easy as just deleting files.
