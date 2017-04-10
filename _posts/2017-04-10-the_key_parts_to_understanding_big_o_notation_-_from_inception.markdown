---
layout: post
title:  "The key parts to understanding Big O Notation -  from *Inception*. "
date:   2017-04-10 03:58:35 +0000
---


Note: "O" in big O means "Order."
Big O Notation is a way to assess how an algorithm will scale when its input scales. 

For example let's imagine you are inside of [Inception](http://www.imdb.com/title/tt1375666/) (similar to the movie) and you are stuck in *Lymbo*. And there is a door that leads to a dream or to your conscienceness. In order to escape you need to create algorythm Y (a tool to help you get to the exit) to find the right door that leads to your conscienceness. Let's imagine there are 10 doors. You would have to go through 10 or less doors until you found the right door. 
How would algorythm Y perform if you had to go through 10 doors or less? 10 doors aren't a lot so it should be pretty quick. Now imagine instead of ten doors there are 1000. How would your algorythm perform with1000 inputs (doors) to find your exit? This is what Big O Notation tries to answer. The efficiency or performance of your algorythm as your input scales. 

**Algorythm O(1) - Constant remains the same **
Starting with the simplest
Let's imagine algorythm O(1) is a compass that points to the correct door out of the 100 doors. All you have to do is walk to that door. Nothing changes you walk in and escape to your conscienceness. Regardless of how many doors there are if you use the compass and go directly to the correct door the time it takes will remain the same. 


**Algorythm O(n) - The number of input your routine recieves **
Let's imagine you don't have a compass. You only have your two feet. S you have to walk through each door until you find the exit. This scales linearly. The time it takes to find the exit will increase depending on the amount of doors (inputs) you have to search. So you would need to search "n" doors to find the exit. This is O(n). 

**Algorythm O(n)² **
Now imagine there is only one door with layers of other doors inside that eventually lead to the last door (the exit). For each door there is a Guardian waiting. Everytime you enter a door one thought is added to your mind. These thoughts that accompany you are parts of  a puzzle that when added together complete the puzzle and form an idea that you will remember when you exit the lst door to your conscience. If the parts aren't perfectly fitted together your mind won't be able to solve the puzzle when you exit the last door and you won't remember. The Guardians do not want you to leave without the puzzle completed. So they have to compare each piece and fit it with it's counterpart. 
Let's imagine you enter the first door the first Guardian will check your first puzzle piece. Since there is only one there is no need to compare. The Guardian will let you keep it and move onto the next door. Now imagine you are at the 5th door. The fifth Guardian will check each of your four puzzle pices and compare them with the new 5th piece that you received when you entered the fifth door. So 5 puzzle picees will be compared 5 times. Even if the guardian can fit one piece with another before checking all 5 he must still check the remainding two because some puzzle pieces fit better with others. So that would be 5 puzzle pieces times 5 puzzle pieces. Same would apply when you go through the 6th door and the 7th etc. The calculation is always squared. Instead of the door being the input each puzzle piece will be the input or *n* that we calculate. 

**Algorythm O(log₂) - logarithms**
This is the opposite of O(n)²
So if I write n² = y, then I can say log y = 2


