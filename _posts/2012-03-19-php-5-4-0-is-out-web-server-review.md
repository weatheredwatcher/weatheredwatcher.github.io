---
date: "2012-03-19T00:00:00Z"
date_published: "2012-03-19T04:00:00.000Z"
date_updated: "2012-03-19T04:00:00.000Z"
slug: php-5-4-0-is-out-web-server-review
title: PHP 5.4.0 is out!  Web Server Review!
---

Let me start out by saying that I have been waiting for this feature for a while now.  When ever I male forays into the world of Rails and Python I always come back to php just a little bit jealous of the testing servers that come with them.  Well, in just a little bit, I will be jealous no more!! \n First things first.  We must install the latest version of php.  I use Fedora for my development machine, so I am going to give you the steps you need in YUM for doing this.  I recommend a little google search for the easiest way to do this in your distro....actually, you really need to setup a repo that will let you keep to the latest version of php anyway. 

 I use the YUM repo at [http://rpms.famillecollet.com/fedora/16/remi/x86_64/](http://rpms.famillecollet.com/fedora/16/remi/x86_64/). To install the repo into your system type:

    wget http://rpms.famillecollet.com/fedora/16/remi/x86_64/remi-release-16-6.fc16.remi.noarch.rpm
    

Follow that up with a:

    sudo rpm -Uvh remi-release*rpm
    

And then 

     sudo yum --enablerepo=remi-test install php

And that will install the latest version of php and all it's dependencies!! 

 Next we will need a proper test!  Lets install the latest version of CI and see if we can get it up and running!!
For this, I am going to clone the latest CI from git into a new folder and then run the server from there.  If you are following along at home, feel free to use whatever code you want! 

So, to run the server, we cd into our site and type a simple:     php -S localhost:8000 (bear in mind that you can use whatever port you want..and even a server name other then local host...take a peak at the docs) 
![php server in the terminal](https://lh6.googleusercontent.com/-BmMXKe-UoOI/T2d6JIBddpI/AAAAAAAAE8U/y1L5F55NG2s/s968/screenshot_php_server.png)
    php server in the terminal
![server running the browser](https://lh6.googleusercontent.com/-TUbOwsrfz5A/T2d8qZPdkcI/AAAAAAAAE8w/A1JiuQyr6P8/s556/screenshot_uzbl_localhost.png)
    php server in the browser
So, first impressions are pretty cool!  It's time to get back to work, but I will be playing some more with this I assure you!!
