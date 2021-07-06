---
date: "2010-02-18T00:00:00Z"
date_published: "2010-02-18T05:00:00.000Z"
date_updated: "2010-02-18T05:00:00.000Z"
slug: everytime-i-look-at-ruby-i-just-have-to-say-wow
title: Everytime I look at ruby I just have to say "Wow"
---

\n    So here is the background:  At our last Mac User's Group gathering, my good friend and Ruby on Rails Jedi, Jed Schneider in the process of his talk showed a pretty nifty little ruby script that created a single file that repeated the same line over and over again a set number of times.  It was pretty sweet.  Fast forward to almost a week later and I am having to create a massive amount of test data for some final testing.  I need to create a grand total of 61 csv files, all with different names but that contain the same five email address (as well as some other information)  So I decided to try and implement a solution in Ruby, drawing on the concepts of Jed's script. [Ian](http://www.iangreulichonline.com)and I sat down for a spare second and knocked out a little script that does just that.

Surprisingly enough (or un-surprisingly enough if you are a ruby programmer) that's all she wrote.

The line 1.upto(61) relies on the fact that in Ruby everything is an object.  We want to output the numbers 1 through 61.  We use |i| to catch the output into a variable i then use File.open to create a file called test1.csv through test61.csv.  For each file we want to write some lines.  Each line can be written with file.puts.  Notice that we pass the name of our file to a variable called file with |file| just like we did for i above.

So after I created 61 csv files with different names but the same data, I decided to check and see if ruby could help me with any of the other test data generation.  Of course it could!

The next little bit of code is really just based on the code above.  Instead of writing the same line of data to 61 different files, I wanted to write 61 different email address to one file, using the same base.  

Considering that this is pretty much the same as the previous script, only inside out, I'm sure you can figure it out without much effort.  

This Ruby Wow Moment was brought to you by the Letter R and the Number Awesome

Questions? Comments?  Any ruby Jedi's out there that can see of a better way of doing this?  I await your comments with much anticipation!
