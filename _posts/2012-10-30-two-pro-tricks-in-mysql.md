---
date: "2012-10-30T00:00:00Z"
date_published: "2012-10-30T04:00:00.000Z"
date_updated: "2012-10-30T04:00:00.000Z"
slug: two-pro-tricks-in-mysql
title: Two pro tricks in MySQL
---

Chances are you use some form of gui when you interact with MySQL.  Imaright?  I don't.  I prefer to use the command line tools for pretty much everything that I do.  MySQL is no exceptions.  That being said, these two tricks are very helpful, even if you normally use a gui...in fact, the second trick will work in the gui's as well, 'cause it is really just some really cool sql.\n The first thing that we are looking at is how we end a sql statement. Typically, you use the ';' to end your statements like this:

    SELECT * FROM table WHERE type="device";
    

A pro trick is to use "\G" like this:

    DESCRIBE table\G
    

So what is the difference?  Well, "\G" gives a much easier to read print out of the data then the traditional tabular form.

    mysql> select * from cms_phone_features where entry_id=4766\G 
    *************************** 1. row *************************** 
    entry_id: 4766
      row_id: 18973
      row_order: 0
      style: Touch Screen
      keyboard: Virtual
      camera_front: 1.3-megapixel
      camera_rear: 3.2-megapixel
    1 row in set (0.00 sec)
    

The second trick that we are going to show today is some cool sql.  So here is the scenario:  You are dealing with a very large database with a massive amount of tables.  Now, all you really want to deal with are a subset of these tables.  They all happen to be prefixed 'ds_'.  So how can we list just these tables?

    show tables like 'ds%'\G
    

The 'like' term will list tables like whatever string you follow with.  The string must be in '' and the % is a wildcard that matches anything.

Not so bad huh?  As I am trying to strengthen my dba chops, you will be seeing database specific entries mixed in with my programming entries.
