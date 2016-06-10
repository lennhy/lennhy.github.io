---
layout: post
title:  "**Procedural Ruby - all you need is a little intuition "
date:   2016-06-10 04:54:09 +0000
---

***
After getting my hands wet with procedural ruby I encountered problems that required more than just knowledge but more so intuition. The kind of intuition that involved using the knowledge I had and applying it in a less obvious way and almost counterintuitive. This became clear to me after spending hours on a single problem that I would have solved faster if I knew how to intuitively use placeholder values to switch and change around variables to my liking. 

When you want to return a value that you put into a method a named placeholder allows you to change those values without hard coding it. It may not seem so but this is a very useful and powerful tool. 

While coding I needed to solve a problem by comparing each value in an array to one another in order to sort them from highest to lowest, without using any ruby methods like sort etc. This required iteration but there wasn't a clear enough way to compare the values in the array without hardcoding a starting value. Here is the code:

`def key_for_min_value(name_hash)

	#set initial placeholder variables to compare to each value everytime you itereate
 
	min_value = false
	min_key = false
 
	if name_hash.empty? 
		return nil
	end
 
  name_hash.each do | key, value | #start loop
		if min_value == false  #check to see if it's exactly identical to false first since it is not it will take the value of the first iteration
			min_value = value  #min_value is now equal to false
			min_key = key
						# we don't want min_value to stay false so we reset it below
		elsif min_value > value  # reset and assign the current value to the min value to compare and assign  the lowest value
			min_value = value
			min_key = key
  	end  
	end
 
 return	min_key
 
end`

Fortunately, in Ruby you can easily change the value of variables so I simply set a placeholder variable equal to false or nil so that when the method iterated it would compare against the placeholder value. If the proceeding new value from the iteration was lower than the placeholder variable, then the value of the placeholder would be reset to the new lesser value in the iteration. This would continue until the end of the iteration where the lowest value in the iteration would be set to the new placeholder value or the "min_key."

Another valuable lesson I learnt was that using tools like IRB and Pry would help me to overcome unseen obstacles that were again not as obvious and sort of hidden in a sense. These tools give me a clear picture as to what was happening behind the scenes of my code. For example, seeing the result of each value after it iterated during an iteration would be easier with pry or IRB. Breaking down code into parts is important when building code to solve a problem and it is just as important for checking problems and fixing your code. Many times I was stuck on a problem and didn't know why or how to get myself unstuck. IRB allowed me to test my code and what I really liked about Pry was that i got to see the value of the objects I had in my code so I knew If I was returning the correct value or not. It's sort of an RSPEC test but without having to write an RSPEC test. 

When newbies first learn Procedural Ruby it can be challenging especially when the problem requires a somewhat counterintuitive approach. However, once you use the tools I mentioned above road blocks soon become stepping stones and stepping stones soon become your wings and eventual super powers.
