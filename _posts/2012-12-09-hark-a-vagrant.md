---
date: "2012-12-09T00:00:00Z"
date_published: "2012-12-09T05:00:00.000Z"
date_updated: "2012-12-09T05:00:00.000Z"
slug: hark-a-vagrant
title: Hark A Vagrant
---

Ok, so we start out today with a double reference!!  First we are paying homage to the incredibly funny web-comic Hark! A Vagrant.  If you are a history nerd certainly check it out!!(Hark A Vagrant).  But we are really talking about the really cool vm utility Vagrant.  In a nut shell: You download/load a VM packaged as a vagrant box.  It is loaded and run in the background using Virtual Box.  Once properly setup, you can ssh into it as well as view it's web contents in a browser. (using port forwarding).  It can also be provisioned using Chef or Puppet. \nWhat this means is that you can configure a custom server environment ready for your entire team and they can download it ready to go....or if you want to save on downloadings..you can create a base system and then write a provisioning script that installs EVERYTHING that is needed.  It's a pretty sweet little setup!  It's also a great way to play around with different languages/environments on the fly.  I built my own base Debian Wheezy box and I have been using it to play around with Node.js without compromising my work environment! 

Some link love:

- [Vagrant](http://vagrantup.com/)
- [Chef](http://wiki.opscode.com/display/chef/Home)
- [Puppet](http://docs.puppetlabs.com/pe/2.0/cloudprovisioner_overview.html)

I must say, that despite what I have heard, it was very easy to get setup and going...but that might also be because I am already using Linux, and so all the tools are running native.  I'm not going to go through the step by step 

here..the site does a good job of that.  I thought about linking my Wheezy box, but it is pretty big (700+ mb image).  I still might, and post it as an update.  Regardless, it is fairly easy to get started...they link to a base 

Ubuntu box in the instructions.  Once you have practiced deploying a server, the provisioning tool is a lot of fun.  I have been using puppet and it is fairly easy to use.  The fun trick was creating my own image.  You have to 

build a base VM in Virtual Box.  Make sure that you do not install any kind of GUI/Window Management on the box as it is not needed!!  You can actually customize the image before you package it...add users, software, even sites. 

If you are packaging for a team and size is not a major issue (say, if you are going to distribute it on a network share internally or something)you can forget about provisioning and load everything manually.  Otherwise making 

good use of provisioning can bring the size down.  Once you have built the VM, it is a single command line in Vagrant to build the box for you. 

Well...that's all that I have time for today! If you are interested in my base Wheezy box let me know in the comments and I will make sure and post it GitHub or something (that seems to where are the big kids are posting their 

vagrant boxes...)
