---
title: "Lnav to View Docker Logs"
date: 2020-02-13T11:19:38-05:00
draft: false
tags: [linux, docker, dev, devops]
type: "post"
---

So, using this as a means to document stuff that I like so that I do not
forget it later!  I find a really cool tool last year called lnav. It's
a logging tool for the command line.  A neat thing about it is that you
can pipe docker logs through it and get a great, easy to navigate system
for parsing the logs....it's a bit like Papertrail.

So ```sudo apt install lnav```

And once you have it installed, just running ```lnav``` loads the local
logs.  Or ```sudo docker-compose logs | lnav ``` will pipe the Docker
logs through lnav and give you a pretty little stream!
