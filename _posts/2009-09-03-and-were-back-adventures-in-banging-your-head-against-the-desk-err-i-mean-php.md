---
date: "2009-09-03T00:00:00Z"
date_published: "2009-09-03T04:00:00.000Z"
date_updated: "2009-09-03T04:00:00.000Z"
slug: and-were-back-adventures-in-banging-your-head-against-the-desk-err-i-mean-php
title: And we're back.....adventures in banging your head against the desk..err...I
  mean php
---

\n    Okay, I'm back!  After nearly a month out of the workforce, I am currently back at the old grindstone.  I am working on a contract with The State Media Company.  It's great to be back at coding again professionally!  Anyway, I got two tid bits to share with all.  The first involves sessions and the second headers.  The app that I am developing (can't get too much into it right now but I'll re-work some of the code for here laters) uses a lot of session work.  You MUST load the session before you do ANYTHING else.  Don't do it, it won't work!  About headers......this is some cool stuff.  I use the code:

    header("refresh: $sec, $url);
    

to reload a page after the form is validated.  Here is the catch: any program output and the header will not load.

    echo("Form didn't Validate");
    header("refresh: $sec, $url);
    

This code above will result in epic fail.

Another thing that I learned: I really needed to be using Netbeans for this.  My code was working yesterday, today not.  I had made changes to the code and had I been able to use internal versioning of Netbeans, I wouldn't have banged (too much) against the desk!
