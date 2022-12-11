---
date: "2021-12-27"
date_published: "2021-12-27"
date_updated: "2021-12-27"
slug: why-i-use-vim
title: Why I Use Vim
tags: [linux, vim, dev, php]
type: "post"
---

So, why do I use VIM?  I had a great example of this just today and I
thought I'd share!!

For work I am needing to validate a CSV file with a massive header!  So
I pulled the header and pasted it into my class.  I needed to convert
the header to an array.  So I needed to split each line with a ',' and
surround each field with ''s.  In VS Code on a Mac lots of going back
and forth.  With Vim, I can just use '.' to repeat the ',' element...so
'w,w' brings me to the start of the next word and a '.' repeats the ','
and inserts a carriage return as well.  It took me about ten seconds to
do that.  Then for the surround word, I created a macro to surround a
word this single quotes and then I just ran it against the list...again,
minutes is all it took to surround all the items with single quotes!




