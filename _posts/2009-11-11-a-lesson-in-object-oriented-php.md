---
date: "2009-11-11T00:00:00Z"
date_published: "2009-11-11T05:00:00.000Z"
date_updated: "2009-11-11T05:00:00.000Z"
slug: a-lesson-in-object-oriented-php
title: A lesson in Object Oriented PHP
---

\n    I've casually referenced oo php recently.  So here I am going to give a more formal tutorial on object oriented php for anyone out there who is unaware of OO programming in general.  If you are familiar with OO programming, you will understand everything immediately.

The first thing that we are going to create is a class.  This class will allow us to make an object based on that class.  

Think of a class as a cookie cutter.  We make objects from this class.  Each object should be the same as all the others, except for the data that we place into the object.

    class Car
    {  
       var $make;      
       var $model;
    }
    

Here we have defined our class "Car" as well as two variables, "make" and "model."  Now we have to make some setter methods for our class

    function add_make($name)
     {  
        $this->make = $name;
     }
    

function add_model($name)
     { 

        $this->model = $name;
     }

These two functions set the variables for the make and model.  The next functions are called getter methods, because they 'get' the data from the object.

    function get_make()
    {  
       return $this->make;
    }
    
    function get_model()
    { 
        return $this->model;
    }
    

Next we will build a driver file to test out our class.

    require_once('carClass.php');
    
    $myCar = new Car;
    $myCar->add_make("Ford");
    $myCar->add_model("Explorer");
    
    
    echo $myCar->get_make();
    echo $myCar->get_model();
    

Basically, we first create a new object "Car" called myCar.  Then we add a make and a model to the Car object.  Finally, we echo off the variables to see what we have set them as.

Here is the complete class file for you to study.  For homework, make a getter and setter method for color and then add it to the driver file.

carClass.php: 

    class Car
    { 

       var $make; 

       var $model;
       var $color;

     function add_make($name)
     {  
        $this->make = $name;
     }
    
     function add_model($name)
     {  
        $this->model = $name;
     }
    
     function get_make()
     {  
        return $this->make;
     }
    
     function get_model()
     {  
         return $this->model;
     }
    
    
    }
    
