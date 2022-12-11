---
date: "2009-12-24T00:00:00Z"
date_published: "2009-12-24T05:00:00.000Z"
date_updated: "2009-12-24T05:00:00.000Z"
slug: classless-object-oriented-javascript-programming-lesson-1
title: Classless Object Oriented JavaScript Programming (Lesson 1)
---

\n    I've been exploring Object Oriented Programming with JavaScript.  It's been an interesting experience because JavaScript is not really a traditional Object Oriented Language but rather a prototype language.  Prototype is  a type of object oriented language that is classless.  In a prototype language, inheritance is achieved by reusing prototypes of objects.  It is a fascinating model.    So let's get started on our exploration of class-less object oriented programming
The first thing of note is that we are not actually creating a class (note:Classless Object Oriented JavaScript) Essentially, all we are going to do is create the equivilent of a constructor.  Our constructor will have an alert box that fires whenever we create a new object, as well as some basic properties of our car. 

Now, when ever we create a new Car prototype, we get this alert.  It's a great way to make sure it's working!  Now lets add some variables to our class.  For a car we will need a make a model and a color.

Now, after the function, we need to do a little bit more.

This sets the default value of the make, model and color.  It also introduces us to the prototype property.  Later, we will use this prototype property to create our sports car class that inherits the Car class.   Now we are all set to create some cars!

The cool thing about JavaScript is how methods are handled.  We actually define a function with the prototype property.  Just remember that we need to use 'this' to access the variable in the class.

Go ahead and play around a bit with this.  Next, we will talk about how you can use prototype to create another class that inherits another class.
