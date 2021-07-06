---
date: "2010-02-02T00:00:00Z"
date_published: "2010-02-02T05:00:00.000Z"
date_updated: "2010-02-02T05:00:00.000Z"
slug: custom-error-handling-with-php
title: Custom Error Handling with PHP
---

One major headache in any major project is how you are going to handle the errors.  For starters, it is not a good idea to show errors on a live, production site.  But on the other hand, you don't want your system to blow-up and you not know what has happened.  There are lots of ways that you could handle this situation.  This is just the way that I am handling it on my current project.  Background: My current project is a behind-the-firewall-intranet application.  It is not accessible for out side the office.  So it's safe to leave error messages on...except for the fact that no one wants to see the error messages except for me.  I was given the task of making errors as terse and minimum as possible in their output, while at the same time recording more detailed information in some kind of log file that could be seen my anyone who wanted to know more information.

*set_error_handler*

The first bit of magic that I used to accomplish my goals is the set*error*handler tag.  Essentially, we create a function, when passed the error codes, out puts user friendly message and can even keep the program running after a near fatal error.  

We pass the function the error number, string, file and line number.  This allows us to create useful output.  We can even branch out further, displaying different messages depending on the error number.

The only thing we have to do to make this work is to place the function set*error*handler("myErrorHandler"); on every page that you want this custom error handling to occur.

The next step was to figure out a way to store the data without actually displaying it.  To do this, I wrote a simple class file that writes data to a database.  You can just as well write the data to file.  The principle is the same. 

The new function for handling the errors looks like this:

This way, I can use simple checks to see if processes for successful or not, writing any messages to a log to check later.  I also dump the contents of the log in a table so that if a user throws an error, they could potentially see what that error was.  

Feel free to use this code in your own projects.  You can always change it to write to a flat file, csv or xml file.
