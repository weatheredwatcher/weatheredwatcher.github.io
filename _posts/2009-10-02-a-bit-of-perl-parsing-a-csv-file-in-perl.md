---
date: "2009-10-02T00:00:00Z"
date_published: "2009-10-02T04:00:00.000Z"
date_updated: "2009-10-02T04:00:00.000Z"
slug: a-bit-of-perl-parsing-a-csv-file-in-perl
title: a bit of perl - Parsing a CSV file in perl
---

\n    I've been working on a solution that takes uploaded CSV files and parses them to process the data (in my case ultimately it will live in a database)  Here is some sample code that I put together in my sand-box that takes care of parsing the data in to an array.  *DISCLAIMER* This is not a newbie tutorial.  If you don't already understand perl and oo programming, you might be a little bit lost.  This solution requires the Text::CSV module from CPAN (Right here)

    #!/usr/bin/perl
    # Sandbox application for testing perl script CSV parse
    
    use strict;
    use warnings;
    use Text::CSV
    
    my $csv = Text::CSV->new();
    
    open (CSV, ") {
                    next if ($. == 1); 
                    if ($csv->parse($_)) {
                        my @columns = $csv->fields();
                        print "tBusiness Name: $columns[0]ntContact: $columns[1]ntMessage: $columns[11]nn";
                        print "t----------------------------nn";
                } else {
                        my $err = $csv->error_input;
                        print "Failed to parse line: $err";
                }
    
    
    
        }
    
        close CSV;
        exit;
    

Okay, lets break this down a bit.  The first thing that we do (after loading all the mods) is to create a new csv object.

    my $csv = Text::CSV->new();
    

Then the while() line iterates through the file.

This code pareses and displays the data:

    if ($csv->parse($_)) {
            my @columns = $csv->fields();
            print "tBusiness Name: $columns[0]ntContact: $columns[1]ntMessage: $columns[11]nn";
            print "t----------------------------nn";
    

The thing that you need to keep in mind for later is that all the data is stored in an array called @columns.  We can access the data by specifying which array item to use.  Like most languages, we use [n] to tell which item to use.

Next time I will write about how you can take this parsed data and insert it into a database.
