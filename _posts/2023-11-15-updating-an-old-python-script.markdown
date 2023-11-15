---
date: "2023-11-15"
date_published: "2023-11-15"
date_updated: "2023-11-15"
slug: "updating-an-old-python-script"
title: Updating an Old Python Script
tags: [wordpress, dev, python, ops, apache]
type: "post"
---


I wrote a script years ago that let me spin up a WordPress instance, complete with vhost file.  I wrote it in python and it was , at least for me a pretty
impressive feat!  Recently, I've been going over some of my old code and giving them a bit of a dust off.  I've made a few changs to the code that I feel makes
marked improvements on it...albiet small!

Here is the code in it's entirety:

{% gist 14dd0d50ee3e876746ff %}

To break it down a bit, the main change was in moving to f-strings.  This makes the code cleaner and easier to read.  Instead of ```%s``` in all the spots that needed dynamic data
and then ```%(domain, domain, path, path, domain, domain)```, I can just include the variables directly with ```{var}``` instead!
