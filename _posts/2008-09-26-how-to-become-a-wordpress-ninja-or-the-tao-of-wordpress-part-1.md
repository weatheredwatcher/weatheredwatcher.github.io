---
date: "2008-09-26T00:00:00Z"
date_published: "2008-09-26T04:00:00.000Z"
date_updated: "2008-09-26T04:00:00.000Z"
slug: how-to-become-a-wordpress-ninja-or-the-tao-of-wordpress-part-1
title: How to become a Wordpress Ninja (Or the Tao of Wordpress) Part 1
---

\n    I've spent the past two months now hacking Wordpress into peices.  I've decided to share the wisdom that I've learned along the way here at my new Wordpress Blog.
Here is a bit of background:

I was hired by a local Technical College to build a podcasting server using Wordpress.  Everything that I did had to be carefully branded to the college and also built keeping in mind that a majority of the users would be very much not computer literate.

First of all,  I created a gateway file that would direct users from the school website into the podcast blogs.  I created two pages, one for students and one for staff.  For starters, the student page had to contain a drop-down menu listing all the instructors on the site.  I populated the drop down from the database of wordpress users (oh..btw, this is wordpress MU)

    $result = mysql_query("SELECT display_name, user_nicename FROM wp_users");
    while ($row=mysql_fetch_row($result)){
        $blog_id[]=$row[0];
        $blog_domain[]=$row[1];
    }
    
    mysql_close($db);
    

This first bit of code creates a query object and dumps the content of two rows in to two arrays. 

    $blog*id and     $blog*domain

The second part of this magic uses some really nifty php code that I like a lot:

foreach( $blog_id as $key => $v)

This little gem takes the array, places the array key into     $key and the array value into      $v this allows us to do some crazy manipulation....that is, I can pull from the second array in my foreach loop.

This took care of the students side.  For the staff side, I read countless Forum entries on "customizing" the login....there is even a plugin to help you with this.  All of these methods to me seemed inadequate for my purposes, so I copied the relavant code from wp-login.php and stuck it in my own file.  Viola!  A truly custom Wordpress Login!!

Check back later as I get even dirtier with the mods!!

weatheredwatcher
