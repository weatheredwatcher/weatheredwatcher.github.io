---
date: "2010-12-03T00:00:00Z"
date_published: "2010-12-03T05:00:00.000Z"
date_updated: "2010-12-03T05:00:00.000Z"
slug: javascript-design-patterns
title: JavaScript Design Patterns
---

\n    I was thinking recently about design patterns and realized that though I am familiar with them and their uses in php and ruby, I have never used them in JavaScript. I quick search found some code on Wikipedia for a singleton...but that is arguably not a very useful pattern. I looked under the entry for Factory Method patterns and found that while the singleton had an example for nearly every language under the sun, the Factory Method Design Pattern only had an example in Java (and a rather tasty one at that!) So there I was, the challenge was on. I decided to try and implement a Factory Method Design pattern in JavaScript without looking, using the Java source as my guide point. So here it is, using prototype in all it's glory. I present to you, a JavaScript Factory Method Design Pattern:

Now, the point of a Factory pattern, is the creation of various objects types. So, as you can see I selected the pizza. I create three pizza types, all of them extend a base pizza class. Cheese, Pepperoni and Deluxe. Now, each type prints out a lable telling us what it is once it is created. FYI I did this in jsfiddle and you are more than welcome to play around and experiment...this is why I used jsfiddle. :) 

The factory method uses a simple switch to figure out which object we want and then create it. I use an array to load the three types and some main body code to run through the array and fire off the factory method for each type. It's a rather smooth little test if I say so myself! 

Now, please feel free to pick me apart and offer up suggestions or comments! This is done entire as a learning process for me, so I do not give any assurances that this is the best way to implement such a pattern. Please let me know if you have a better way!
