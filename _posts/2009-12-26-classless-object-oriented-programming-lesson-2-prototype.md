---
date: "2009-12-26T00:00:00Z"
date_published: "2009-12-26T05:00:00.000Z"
date_updated: "2009-12-26T05:00:00.000Z"
slug: classless-object-oriented-programming-lesson-2-prototype
title: Classless Object Oriented Programming (lesson 2 prototype)
---

\n    Merry Christmas!  Today is also my daughter's fourth birthday.  She sat with me and helped me prepare for this lesson..  We created a Girl class and a Girl object called Luba.  Then we made her sing!  It was alot of fun.  I don't think she understood Object Oriented Programming, but it was a great start.

I've decided to do a little bit more on prototype and why this is a pretty cool concept.  For starters, lets take a look at our previous class, Car.  What if I took a look at my class and decided that it was not enough.

    function Car (make, model, carColor){
        this.make = make;
        this.model = model;
        this carColor = carColor;
    }
    

What if I needed to add another property, say engineSize, I could add a line in my class and hae engineSize, but what if I only need a property if a certain condition occurs.  This is where the prototype property is a great thing.  I highly recommend you use firebug and give this code a try.  It might not be programmatically practical, but it is a great exercise.

Go ahead, open firebug and switch to the console.  I highly recommend that you switch the command line to Larger Command Line - this gives you a large command block on the right hand side.  Now, enter the following code into the command line and run it.

    function Car(make, model, carColor){
        this.make = make;
        this.model = model;
        this.carColor = carColor;
           alert('Smell that new car smell!');
    }
    

Once you see the results in the console, clear the command line and enter this code

    var myCar = new Car('Ford','Mustang','Red');
    

You should have seen the prompt that new car was created.  So now we have a new object that we can play with.

Now lets define a new property and call it carsMPH.

Since we are running this is the console, we really can't re-do the class, so we will use the prototype property.

    Car.prototype.carsMPH = 'How Fast';
    

If you type alert(myCar.carsMPH); you will get 'How Fast'.  This is because we have not set a unique value for this variable and so the default is used.  We can define the cars speed with 

    myCar.carsMPH = '30';
    

Now we follow up with the previous alert and we now get 30.  Here we come on another issue.  Having to enter in the alert phrase over and over again is a bit annoying.  So lets create a method that will display the cars speed.  Once again, since we aren't able to change our class after it's run, we will use prototype.

    Car.prototype.speed = function(){
        alert('You are going ' + this.carsMPH + ' miles per hour');
    }
    

Now all you have to do is type myCar.speed(); and a dialog box pops up and shows you how fast our car is going.

So there you go!  That was a bit more indepth overview of prototype and classless object oriented programming in JavaScript.  With that background out of the way I think that we can move on to some more complex concepts.
