---
date: "2011-09-15T00:00:00Z"
date_published: "2011-09-15T04:00:00.000Z"
date_updated: "2011-09-15T04:00:00.000Z"
slug: managing-virtual-hosts-on-a-mac
title: Managing Virtual Hosts on a Mac
---

One of the coolest tricks that you can do with Apache (ok, so not the coolest but certainly one of the most useful!!) is setting up VirtualHost.  VirtualHost allows you to host many different domains on a single IP address.  As complicated as it is to setup on a real web server, it is even more so on a development machine that doesn't have an IP address or domain registered to a DNS.

In order to set VHosts up on a server, you must:

    *Activate Virtual Hosts
    *Add your Virtual Hosts File
    *Reboot Apache
    

Steps 1 and 2 require some editing of the httpd.conf file and a vhosts file.  With that being said, if you want to do this on your local 

dev machine, you need to add another step: Add the VirtualHost to the hosts file.  Once you make sure that VirtualHosts are activated 

the rest of the code involved is something like this:

httpd_vhost.conf:

     NameVirtualHost *:80
    
     <Directory "*/path/to/site/">
     Allow From All
     AllowOverride All
     </Directory>
    
     <VirtualHost *:80> 
    
         ServerName "sitename"
         DocumentRoot "/path/to/site"
     </VirtualHost>
    

hosts:

     127.0.0.1  sitename
    

And you will have to do these two things for each and every site that you want to use with VirtualHost.  Fear not, there is another
way!  I found an app called VirtualHostX that takes care of everything for you.  It costs.  It costs a little bit more than I would want to spend 

for taking care of these two tasks for me.  But it is a helpful app none-the-less. Get it here

In addition to this, if you also install PassengerPhusion, your rails apps will also run without you having to use web-brick!

With all of this talk, I have made the decision that the very first cross-platform app that I intend to build for my new software house 

will be one that does just this.  I will write it first in Objective C and then port it to Linux and then in C# for Windows. (I hope that I can port 

the objective c code straight into Linux...but if not I can use C# for both Linux and Windows) So stay tuned for the free beta application!! 
