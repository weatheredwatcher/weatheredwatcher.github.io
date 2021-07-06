---
date: "2009-07-20T00:00:00Z"
date_published: "2009-07-20T04:00:00.000Z"
date_updated: "2009-07-20T04:00:00.000Z"
slug: a-simple-mvc-framework-part-2
title: a simple MVC framework part 2
---

\n    In this post we will talk about the requirements of having two views of the same data in PHP.  First of all, we must do a little bit of modification of our controller

    $id = $_GET['id'];
    $view = $_GET['view'];
    
    if (!isset($id)){
    
        $id = "home";
    
    }
    
    include($id.'.php');
    

As you can see, we added a single line to this code:     $view = $_GET['view']; 

This allows us to specify what page view we are going to use at this moment.

It would be passed to the url like this: [http://weatheredwatcher.com/ccd_framework/?id=home&view=user](http://weatheredwatcher.com/ccd_framework/?id=home&amp;view=user)

Of course, we are not done.  There is more code that is needed to make this work.

For staters, we divide the page into two functions.  One function contains the code for the user view and the other contains the code for the edit view.  The switch is made via the URL and is controlled like this:

    $page_id ='home';  //this is where you can set exactly what this page is.
    $view = $_GET['view'];  //this pulls the view from the URL
    
         if($view == 'user'){  //here we check if this view is set to user
    
            user($page_id);
         }
         else{
             if($view == 'edit'){  //here will check if this view is set to edit
    
                 edit($page_id);
         }
        else {  //this final else creates a catch-all for any errors that might exist
    
    
    
             error_page();
        }
    }
    

As you can see, three functions must be defined.  A user function, an edit function and an error_page function

Next time we will go over the code needed to add database dynamics to this code.  That is, we will store the content of the page in a datasource and we will show how to both display and edit this data using our views.
