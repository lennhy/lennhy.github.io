---
layout: post
title:  "A more visual approach to learning Object Orientated programming."
date:   2016-06-30 18:16:34 +0000
---

![](http://vignette1.wikia.nocookie.net/spongebob/images/f/f2/Spongebob_12_big_eyes.jpg/revision/latest?cb=20120113194744)

Tired of writing the same code over and over in the console or in IRB ...why not use pry? Maybe for some they can read about object oriented concepts, write a few lines of code and be able to quickly grasp the concept. But if you are like me and you learn better by seeing and doing then you will want to hear about my experience learning object oriented programming in ruby. Unless you have the ability to see through lines of abstract code with kyrptonian like xray vision and picture every object, instance or variable that’s hidden inside then by all means you are a programming Jedi. 
But for me to completely grasp the more abstract concepts and see the code that’s being executed and calculated under the surface I have to pierce the veil of abstraction and pier under the abstract layers of code that can’t be seen on the surface of the code editor. If look at Figure 1. Line of code at first glance you may think that this is a method that’s calling on some code that’s doing something and this would be right. However, there is obviously more to the code than what you are seeing here. "If I cannot see in my head I cannot conceiveth."

<a href="http://imgur.com/9IKZJ54"><img src="http://i.imgur.com/9IKZJ54.jpg" title="source: imgur.com" /></a>
*Figure 1 hmmm… I think I know what’s going on here but do I really…?*

So to jump this hurdle I turned to simple developer tools. One of which is called ‘pry’: For Ruby "an IRB alternative and runtime developer console” that allows you to directly engage with any line of code. You do this by first typing a command in the editor “binding.pry” and typing in a function, or call to a method in the terminal to see what ruby is doing with the code behind closed doors. See Figure 2. Below.

<a href="http://imgur.com/Ynx0Ub9"><img src="http://i.imgur.com/Ynx0Ub9.jpg" title="source: imgur.com" /></a>
 
*Figure 2 Behold there was pry*
*https://rubygems.org/gems/pry/versions/0.10.3*

There are also others like this. For example, in the ever familiar language of the web JavaScript there are helpful developer tools in the chrome and Firefox web engines. For chrome you have its inspect extension and for Firefox there is firebug and many others for the other browsers.
To further illustrate the power of these developer tools more specifically ruby’s pry. I entered a call to all the instances of the class object Artist as seen in Figure 3. below. 

<a href="http://imgur.com/v02M66a"><img src="http://i.imgur.com/v02M66a.jpg" title="source: imgur.com" /></a> 
*Figure 3 pry in action...ohhyeeaa!!* 

What do you think is happening here? It’s not that hard to figure out as there is no abstraction just a a few instances of an arrays, an instance variable that stores a string “@name” and an Artist object in which all of this is stored. This is a much more transparent view of the first set of code I introduced you to at the beginning of this blog wasn’t it? That’s the power of using developer tools whether it be for the web or in your mac or pc’s terminal. But wait I’m jumping too far ahead before I even explain what exactly is happening in the written code. To this we must first go back to figure one and evaluate the code. 
 
<a href="http://imgur.com/9IKZJ54"><img src="http://i.imgur.com/9IKZJ54.jpg" title="source: imgur.com" /></a>  
*Figure 1.2  Now I have x-ray vision*

Above Figure 1.2 we are presently in the Importer class and code in the “Import method” which is simply using a method (new_by_filename) from another class called “Song” to construct its own method for the Importer class. Here is a larger view of the overall picture below.

<a href="http://imgur.com/9IKZJ54"><img src="http://i.imgur.com/9IKZJ54.jpg" title="source: imgur.com" /></a>
*Figure 4 May the force be with you.*

In Figure 4. our Atom editor has two tabs open. The first is for the Importer class and the second is the Song class. In the code when we type binding.pry it sends a message to the terminal that stops everything up to that point in the code where we type binding pry.  In the terminal we see exactly where we stopped. Now we can type in a command and see exactly what ruby is doing. In this instance it is storing an instance of the Song class which hold several instances of the artist, name and songs variables or instance variables. 
This is where most of the collaboration takes place between the two classes.
This is one way to learn object oriented programming. 

Although there are other alternatives regardless of whatever object oriented background you come from this can be helpful in understanding object oriented programming.   
Below in Figure 5. the import method is calling on an instance of the  

<a href="http://imgur.com/9IKZJ54"><img src="http://i.imgur.com/9IKZJ54.jpg" title="source: imgur.com" /></a>
*Figure 5. *
