---
date: "2009-11-13T00:00:00Z"
date_published: "2009-11-13T05:00:00.000Z"
date_updated: "2009-11-13T05:00:00.000Z"
slug: object-oriented-php-lesson-3-coding-a-mailer-class
title: Object Oriented PHP Lesson 3 Coding a mailer Class
---

\n    Today we are going to create a mailer class that will assist us in emailing site visitors.  Lets start out buy creating the class and the variables we will need.

    class Mailer {
    
    }
    

Okay, let see.  We will need a sender, a recipient, a subject and a body. Well, if you know anything about email, the sender is set in the header.  So...

    class Mailer {
        var $headers; 
        var $recipient;
        var $subject; 
        var $message;  
    }
    

Now comes the fun part!  Lets build the email!

First we have a from setter: 

    function from($email, $name){ 

         $this->headers[] = 'From: '.$email.' ';
     }
What we have done, is we have appended the required format for the Header.  In php the '.' is what we use to concatenate strings and variable data together.  Also, note that the header is an array.  We could add more than just "From" to the header if we needed to.

Now we add rest of the functions. 

    function add_recipient($email){

        $this->recipient = $email;
     }
    
    
     function subject($subject){  
    
        $this->subject = $subject;
     }
    
     function message($message) {
    
        $this->message = $message;
     }
    

Notice that we don't have any getter methods defined.  Although it's a good practice to have them in your class for a program, it would just add extra stuff to a class that's sole purpose in life is to run a contact me page.  Now, if you wanted to use this in a larger site.  Say a cms or a social portal, you would want to add a bunch more functions to it.  That way you could get all of your email functionality out of a single class file.

But for now, this is enough.  Let's create a utillity function to send the mail.

    function send()
    {  //sends the email
    
        $headers = "";
            foreach($this->headers as $header)
            {
               $headers .= $header.';';
            }
            return mail($this->recipient, $this->subject, $this->message, $headers);
     }
    

Essentially, we add all the header information, then the rest of the data is pulled into the mail function.

And how do we use this?  By now I hope that you understand the layout of my driver files.  So it's your turn.  Create a contact form and use this class to make it work.
