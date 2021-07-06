---
date: "2009-08-01T00:00:00Z"
date_published: "2009-08-01T04:00:00.000Z"
date_updated: "2009-08-01T04:00:00.000Z"
slug: a-simple-php-contact-form
title: a simple PHP contact form
---

\n    Let's take a little break from the MVC concept and look at a simple but effective PHP mailer.  Like the PHP framework, I wrote this mailer from scratch just to show how easy it is to code pure PHP applications.

Lets first look at a bit of PHP "magic." 

    if (isset($*POST['Submit'])){
        send*mail($*POST['subject'], $*POST['message'], $*POST['header']);
    }
     else {
        show*form();
    }

Okay, time to break it down.

    if (isset($_POST['Submit']))
    

This checks the post to see if a submit button was pressed.  If so, it will carry out the code: 

    send_mail("me@mysite.com", $_POST['subject'], $_POST['message'], $_POST['header']);
    

If there was no submit, because the page has just been loaded or an error occured, the following code is ran:

    show_form();
    

The remainder of the program is contained in two functions, both of which we have already referred to.  The first function is simple enough. It contains a single echo statement that creates the html code for a form.

The second function, sends the mail.

    function send_mail($subject, $message, $header){
        $to = "myemail@mysite.com";
        mail($to, $subject, $message, $header);
        echo("Thank you for sending me an email....I'll get back to you as soon as I can");
    }
    

We could convert the POSTS to variables before we use them.

    $subject = $_POST['subject'];
    $message = $_POST['message'];
    $header = $_POST['header'];
    

This allows us to make some modifications to the header.

    from = "From:".$header;
    

This is of course is a very simple implementation of the PHP mail function.  You can find a great deal more information on the PHP website.  Stay tuned for our next installment.
