---
layout: post
title:  "A more visual approach to learning Object Orientated programming."
date:   2016-06-30 14:16:34 -0400
---

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9291847703941010",
    enable_page_level_ads: true
  });
</script>

![](http://vignette1.wikia.nocookie.net/spongebob/images/f/f2/Spongebob_12_big_eyes.jpg/revision/latest?cb=20120113194744)

Tired of writing the same code over and over in the console or in *IRB* ...why not use *pry*? For some they can read about object oriented concepts, write a few lines of code and be able to quickly grasp these concepts. But if you are like me and you learn better by seeing and doing, then you will want to hear about my experience learning object oriented programming in ruby. 

To better understand how the code is being interpretted and processed I have to see what's happening under the hood. I have to pierce the veil of abstraction, looking under the layers of superficial code. If you look at the line of code in Figure 1 at first glance you may think that this is a method that’s calling on some code and doing something with it which is correct. But, there is obviously more going on here than what meets the eye. "What I cannot see in my head I cannot conceiveth."

<a href="http://imgur.com/9IKZJ54"><img src="http://i.imgur.com/9IKZJ54.jpg" title="source: imgur.com" /></a>
*Figure 1 hmmm… I think I know what’s going on here but do I really…?*

So to jump this hurdle I turned to a simple ruby developer tool called ‘pry’: "an *IRB* alternative and runtime developer console” that allows you to directly engage with any line of code. You do this by first typing the following command in the editor *binding.pry *this creates an environment to test the code you put the *binding.pry* in. Then you can type any command, or call to a method in the terminal to see what ruby is doing with the code you are testing. See Figure 2. Below.

<a href="http://imgur.com/Ynx0Ub9"><img src="http://i.imgur.com/Ynx0Ub9.jpg" title="source: imgur.com" /></a>
*Figure 2 Behold there was pry*
*https://rubygems.org/gems/pry/versions/0.10.3*

There are other developer tools like this. For example, for code written in JavaScript, there are helpful developer tools in the chrome and Firefox web engines. For chrome you have the* inspect extension* and for Firefox there is *Firebug* and many others.
To further illustrate the power of these developer tools more specifically ruby’s *pry*. I entered a call to all the instances of the class object *Artist* as seen in Figure 3 below. 

<a href="http://imgur.com/v02M66a"><img src="http://i.imgur.com/v02M66a.jpg" title="source: imgur.com" /></a> 
*Figure 3 pry in action...ohhyeeaa!!* 

What do you think is happening here? It’s not that hard to figure out as there is no abstraction just a a few instances of arrays, an instance variable that stores a string *@name* and an Artist object that contains all of this information. This gives a much more transparent view of the first set of code in Figure 1. That’s the power of using developer tools. Whether it be for the web or in your mac or pc’s terminal it opens up a whole new world of understanding. But wait! I’m jumping too far ahead. I need to explain what exactly is happening in the code that's written. To do this we must first go back to figure 1. and evaluate the code. 
 
<a href="http://imgur.com/9IKZJ54"><img src="http://i.imgur.com/9IKZJ54.jpg" title="source: imgur.com" /></a>  
*Figure 1.2  Now I have x-ray vision*

Above in Figure 1.2 we are presently in the Importer class and code in the *Import* method which is simply using a method (new_by_filename) from another class called *Song* to construct its own method for the *Importer class*. Here is a larger view of the overall picture below.

<a href="http://imgur.com/9IKZJ54"><img src="http://i.imgur.com/9IKZJ54.jpg" title="source: imgur.com" /></a>
*Figure 4 May the force be with you.*

In Figure 4 our Atom editor has two tabs open. The first is the code for the *Importer* class and the second is the *Song class*. In the code when we type* binding.pry* it sends a message to the terminal that stops everything up to that point in the code where we type *binding.pry*.  In the terminal we see exactly where we stopped. Now we can type in a command and see exactly what ruby is doing with the code. In this instance it is storing an instance of the *Song* class which holds several instances of the artist, name and songs instance variables. 
This is where most of the collaboration takes place between the two classes and is a useful way to learn object oriented programming. 

Although there are other alternatives regardless of whatever OOP background you come from this can be helpful in understanding object oriented programming.   

