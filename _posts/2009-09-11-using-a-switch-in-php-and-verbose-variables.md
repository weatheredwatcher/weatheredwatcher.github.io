---
date: "2009-09-11T00:00:00Z"
date_published: "2009-09-11T04:00:00.000Z"
date_updated: "2009-09-11T04:00:00.000Z"
slug: using-a-switch-in-php-and-verbose-variables
title: using a switch in php....and verbose variables
---

\n    You know, the switch is a very useful and often times overlooked workhorse in php.  I find myself using it a lot right now.  I mainly use it to control a applications flow with session managment.  The switch parses a session variable and based on what it is set as, navigates to a certain point in the application.  Lets get right down to the dirty part!
    $myswitch = $_SESSION['form'];

    switch ($myswitch){
        case 'show':
                show_form();       // if the form will not validate, it is resent
                break;
        case 'validate':       //is the form submited, then it is validated
                validate_form();
                break;        
        case 'process':       //is the form validated, then it is processed
                process_form();
                break;        
        default:
                show_form();     //if nothing has happened yet, we show the form
                break;        
    

}

The first line is very important.  What if no session has been saved yet?  If I call the switch on the session variable, it might not exist and throw an error.  The rest is fairly straight forward.  A switch is, in reality just a better organized set of if..else statements.  I could also do something like this:

    if ($a =="this){
        do_this();
    }
    if (a$ == "that") {
        do_that();
    }
    if (a$ == "nothing"){
        do_nothing();
    }
        do_something();
    

Lets compare to:

    $myswitch = $_SESSION['contest_form'];
    
    switch ($myswitch){
        case 'this':
                do_this();
                break;
        case 'that':       
                do_that();
                break;        
        case 'nothing':       
                do_nothing();
                break;        
        default:
                do_something();
                break;        
    }
    

You see the difference?  Granted, my examples here are not to complicated, but it can get pretty hairy.  The biggest reason to use a switch instead of a bunch of if statements is readability.  One thing that I have learned is the best comments are really well formed code. 

Another great way to get around comments is verbose code.  You might type a few extra bits here and there, but ultimately you save yourself (and others) tons of head ache.

If you have a variable that contains say an address, call it $address.  If it is the city, call it $city.  If it's the city from the address block of a user-registration table, call it $address*city*registration.  In the long run, having a very specific naming convention, and more importantly sticking to it, will help prevent a lot of errors in the variables.

I can't count the times certain pieces of code didn't work for me because the variables didn't match entirely.
