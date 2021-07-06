---
date: "2009-10-28T00:00:00Z"
date_published: "2009-10-28T04:00:00.000Z"
date_updated: "2009-10-28T04:00:00.000Z"
slug: writing-an-array-to-csv-in-php
title: Writing an Array to CSV in PHP
---

\n    The CSV format is a very handy format mucking around in data.  In case you have to write some data into a CSV file, here's some code for your parsing needs.  Quite a few classes exist to both read and write CSV files.  I have a class that I use to read csv into a database and another to create a csv file from a database...Both are killer classes that have great uses (and believe me I have used them) but sometimes you want a little bit of a simpler solution.  Just because php can be an OO language doesn't mean that you have to do everything in php the OO way.  The two main functions that we use for csv files are fopen and fputcsv.  If you have never used file handling in php before, do some reading up on the functions first.  

    $file = fopen("file.csv", "x");
    

In this example, we are actually creating a file called file.csv.  The key is in the second variable.  r,w,x are the basics.  Read, Write, Create.

The second part of this lies in creating the file and putting the data into the csv file.  For this, we will use an array.

    $list = array (
        'Order,Customer,Date,Item,Quantity',
        '001, John Doe, 001, 3);
    

This will create the headers and then add a row of data.  We have several choices here.  We can create the headers separately or we can create the entire file all at once.  Depending on how your data is structured, either way works 

Finally, we need to write the data to the csv file.  To do this, we are going to make use of the fputcsv function,

    foreach ($list as $line) {
        fputcsv($fp, split(',', $line));
    }
    

We have put the function in a foreach loop to make sure that each line in the array is read, parsed and written to the file. First we pass the file, $fp.  Then we tell fputcsv what to use for a deliminator and what to to put on the line.

Make sure that you check out the php manual for more details on these funcitons.  There is a lot of power here waiting to be used.
