---
layout: post
title:  "Understanding Javascript Closures"
date:   2017-04-02 21:44:31 -0400
---


Since I was first introduced to the concept of closures I've had a hard time understanding how it works and why anyone would want to use them - probably because there are so many different examples and ways to make it. 

In JavaScript to understand closures you first have to understand scope. 
When a function is created a scope is made within that function. And when a nested function is created inside another funciton a new scope is created within that function.  But with closure  not only does the nested function have access to variables defined in it's own scope but it now has access to any variable defined inside the parent function's scope as well. 

Morzilla Network gives a breif summary:

*Closures are functions that refer to independent (free) variables (variables that are used locally, but defined in an enclosing scope). In other words, these functions 'remember' the environment in which they were created.*

or according to W3schools : 

*A closure is a function having access to the parent scope, even after the parent function has closed.*

What this means is that a closure is a function that can change a variable inside the function that it was created in. And the change is only executed once the parent fuction has been called.


An example is this add function below (Example taken from w3schools).


```
var add = (function () {

    var counter = 0;
		
    return function () {
		
	    	   return counter += 1;
				
		}
		
})();

add();
add();
add();
```

The counter variable is accessible only inside the add function so other functions can't access the counter. But because the anonymous function is nested inside this function it has acccess to the counter variable it will change the counter by incrementing it everytime add() is called. 

To be honest I still get a little confused by this but you have to remember the part where the variable is only changed by the inner function and can't be changed or accessed by other functions outside of the parent scope. Thus the variable is now a private variable that can only be access from within the parent function. 


