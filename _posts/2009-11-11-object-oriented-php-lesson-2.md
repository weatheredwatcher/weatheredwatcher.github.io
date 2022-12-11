---
date: "2009-11-11T00:00:00Z"
date_published: "2009-11-11T05:00:00.000Z"
date_updated: "2009-11-11T05:00:00.000Z"
slug: object-oriented-php-lesson-2
title: Object Oriented PHP Lesson 2
---

\n    Yesterday we talked about some basics in oo php programming.  To day I am going to show you how to extend a class and also how to be utillity methods (functions) in your class.

Yesterday we created a car class called Car.  To today we will extend that class with a sportsCarClass.  Why, you might ask, do we want to do this?  It's simple.  I might want to create several different types of cars. Maybe I want a SUV car object, or a truck object. All of these objects will have certain properties that are the same (make, model, color, ect)  Why wouldn't I reuse the code that I already have?  I could copy-paste, but that would only serve to make my program a whole lot larger than it needs to be.  So, instead, I'll extend my base class to make other classes.  The one that I'll be demonstrating is the sportsCarClass.  We start out by declaring the class and extending the base class, followed by declaring any variables that this new class will be using. 

    class sportsCar extends Car  {
        var $engine;
    }

In case you are wondering, this is in the same class file as our original carClass.  

Next we will define some setter and getter functions for the sports car class.

    function add_engine($type){ 
    
        $this->engine = $type;
     }
        function get_engine(){  
    
         return $this->engine;
     }
    

I don't need to show you how to use this in your driver program.  Give it a go!  You will notice that you can create a new sportsCar and set make, model, color and engine and it will work, even though you don't have any setter functions for make, model or color in your class.  Also, if you try and set the engine type in a Car, it will fail because Car cannot see sportsCar.

At this point, it might be a good time to point out a few items of importance.  Did you notice that in our driver program we call the functions using the class name followed by -> and the function?  We refer to the variables in our class in the same way, but we use $this instead of the class name.  This is important, because we can also use this to pull variables into other function in our class in this same way.

Lets make a utillity function that will display our object in an easy to read format.

    function show_car(){
                echo('Make: '.$this->make.'');
                echo('Model: '.$this->model.'');
                echo('Color: '.$this->color.'');
                echo('Engine: '.$this->engine.'');
    }
    

Next time we will use oo programming to do something infinitely more useful.  We will create a mailer class that can be used in a contact form.
