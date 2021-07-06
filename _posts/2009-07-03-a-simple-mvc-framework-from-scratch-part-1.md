---
date: "2009-07-03T00:00:00Z"
date_published: "2009-07-03T04:00:00.000Z"
date_updated: "2009-07-03T04:00:00.000Z"
slug: a-simple-mvc-framework-from-scratch-part-1
title: a simple MVC framework from scratch Part 1
---

\n    You here a lot about frameworks these days.  There is Zend, Cake, Rails.  The list goes on.  MVC is a big part of this.  I've decided to write a very simple MVC framework that can be easily understood and implemented on any site with very little intrusion.  I'm going to hash it out right here on my blog.  Please feel free to give me your feedback.

The very first element that we will introduce will be the controller element that implements the views.  This is crucial since the views are what drives the site.  Since PHP is an interpreted language, that is the code is compiled at run time on the server, it is not essential to seperate the various elements of the MVC in directories or files.

Our first little bit of code is here (and yes I am sure many of you will recognize and have used it before)

    $id = $_GET['id'];
     if (!isset($id)){
        $id = "home";
     }
    include($id.'.html');
    

For the uninitiated to this simple little trick, an explanation.

    $id = $_GET['id']; checks the url for a variable 'id' and sets it internally as $id.
    

But what if we have no 'id'?  If it doesn't exist (    if (!isset($id))) we set it to 'home'

The next line is the workhorse of this bit of code

    include($id.'.html');
    

This would, in the event of no 'id' being set, load the page home.html into the current page.  So, you might ask what good is this?  Well, you can define a link as     About and when the page reloads, the include statements passes     include(about.'.html'); which loads the content of about.html into the page.  The beauty lies in the page.  Your index.php and contain all your javascript, css and menu elements loaded just once.  Any changes can be made to the one page.  This is the MODEL component of the MVC.  The code above becomes the CONTROLLER component and the pages that are loaded become the VIEW component.  If this all sounds way too simple, it is.  We can't really have a framework this simple.  A few other things have to be taken into consideration.  How can we implement different views of the same page?  For example, how can we implement a user view of about.html and an admin view of about.html?  We have several different ways to approach this dilemma.

Next time we will explore creating a database that contains the page content and how to implement different views of the same data.
