---
date: "2008-11-13T00:00:00Z"
date_published: "2008-11-13T05:00:00.000Z"
date_updated: "2008-11-13T05:00:00.000Z"
slug: drop-that-table-like-it-was-hot
title: Drop that table like it was hot!!
---

\n    Friday night I ran some code on the server and I had made a tiny but fatal error.  I mixed up a pair of booleans and code that should have made one set of seven tables, made thoose seven table over 1000 times!!  My mySql GUI tools were not upto the challenge.  I found a nifty little bit of code on the internet [Check out the original here](http://knaddison.com/technology/mysql-drop-all-tables-database-using-single-command-line-command) that drops all the tables in database.  Drops 'em like they were hot!!

    mysql -u uname dbname -e "show tables" | grep -v Tables_in | grep -v "+" |
    gawk '{print "drop table " $1 ","}' | mysql -u uname dbname
    

You could even do this from the iPhone!!  Here is a breakdown for anyone that is a bit rusty on bash:

    mysql -u uname dbname -e "show tables"
    

This  (where uname is your username and dbname is your database name) run by itself will show you all the tables in the database.  You have to add the option --password=password if the mysql is protected.

You then pipe it twice through grep -v (this show all BUT what you specify in grep.  And here comes the great part....

    gawk '{print "drop table " $1 ","}' | mysql -u uname dbname
    

You use gawk to print out "drop table" and pipe that through mysql again (just repeat minus the -e "show tables")

And viola!  problem solved!!

David
