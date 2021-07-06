---
date: "2015-05-19T00:00:00Z"
date_published: "2015-05-19T21:09:53.117Z"
date_updated: "2015-06-21T03:25:06.296Z"
slug: running-node-js-with-apache
title: Running Node JS with Apache
---

So, in case you have not noticed, I have swapped the blog out again =). It is currently using Ghost, which is coded in Node JS.  I've been playing a bit more lately with Node and I am enjoying it greatly. In setting up my blog, I have learned a few things and I thought that I might share them!

###### PM2

The first thing that we need to talk about is how to run your Node App (regardless of what it is)

Typically when we are playing with node, we run our apps with `node start`

This is obviously not production quality.  Instead, we install pm2 via npm and use this to run a daemon of our app.  

    pm2 start app.js "appname"  
    

That loads the app.  Now you can do thing like:  

    pm2 reload appname  
    

In addition to being able to easily stop and start your application, it will restart in the event of a reboot.

Also, if you are running multiple applications, pm2 gives a lot of handy usage data.

###### Apache

Now comes the fun part.  Since the node app runs on it's own server with it's very own port, we have to setup proxy in apache to run it.  Make sure that you have enabled `proxy` and `proxy_http`.

Then comes the vhost file:  

    <VirtualHost *:80>  
        ServerName blogdomain.com
        ProxyPreserveHost on
        ProxyPass / http://localhost:2368/
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/ghost 
    
        ErrorLog ${APACHE_LOG_DIR}/blog-error.log
        CustomLog ${APACHE_LOG_DIR}/blog-access.log combined
        <Directory "/var/www/ghost">
            Options FollowSymLinks
            Require all granted
        </Directory>
    </VirtualHost>  
    

Now just load the conf file, reload the apache2 service and you should be good to go!
