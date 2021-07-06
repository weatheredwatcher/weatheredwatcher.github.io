---
date: "2014-11-15T00:00:00Z"
date_published: "2014-11-16T04:19:19.000Z"
date_updated: "2015-05-19T00:16:00.472Z"
slug: base64-decode-hack
title: Base64_Decode Hack
---

Ever wonder how a site like Wordpress or Magento get hacked?  It's usually done via a eval/base64_decode hack. 

In php, here is how this works:

    echo 'Hello World!';
    

Run this and you get:

    Hello World!
    

If you take this and run it through `base64_encode()` you get a hash.

Then, we run `base64_decode()` and we get the command back.  We can use the eval() command in conjunction with this:

    eval(base64_decode($string));
    

I've seen strings that decode into scripts that build zip files and executables...all used as payloads in spam based attacks.

The good thing is that you can find this type of hack in your code fairly easily.

    grep -Rn “base64_decode *(” /var/www
    
    grep -Rn “eval *(” /var/www
    
