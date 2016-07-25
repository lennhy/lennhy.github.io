---
layout: post
title:  "Programming Class, Instances and methods in the Streets of New York City. "
date:   2016-07-25 18:03:27 -0400
---




Apart from drawing parallels with the busy hustle and bustle of life in New York City, I use these as metaphors to Illustrate a more visual picture of programming principles. Also aside from my other odd jobs this is indeed the most interesting and the easiest way for people who aren’t savvy in the abstract ways of programming to understand. 
Like an Object Class, New York City has various attributes and methods that are part of the city streets. Taxis (Class Methods) transport passengers, bicycle couriers (Instance Methods) deliver food and messages to customers, and Sports cars (Object Instances) well, they pop out of nowhere!  

Just a reminder:
Classes: are like the streets and highways of New York City. 
Objects Instances: are like the sports cars that pop out of nowhere and speed into the streets of the city.
Instance Methods: are like bicycle messengers, they have a very specific message to deliver through the city.
Class methods: are like Yellow cab Taxi Drivers they never go outside of the city.


Object Instances:
There was a day when I had to deliver my last package with 3% left of my phone’s battery life and 5 miles to cover on my 27 speed bicycle. But more about that later. Near the end of my delivery as I was about to park my bicycle and I heard a sudden roaring sound. I turned and saw what I believe to be a very expensive sports car the kind you only see in movies like the Fast and the Furious, the one’s that you can never remember the name of but look familiar because you probably saw them in a movie. In it were to young guys yelling at something behind them. When I turned to see what they were yelling at, out of nowhere comes six other sports cars all with young guys probably in their twenties slowing to an almost dead stop. In that moment the first car sped off and the other cars followed like a race they were there and then gone in an Instance. 
Instances or Object Instances are like Sports cars: They pop out of nowhere and get instantiated at an instance notice speeding into existence. There only purpose is to be instantiated. We can’t do much with it but race against other instances to see who gets instantiated first. Or until a message is sent to them by an instance Method. Look at the example below:
Lets start off with an illustration of a class in *Figure 1* below:

*Figure 1.*
```
class Song
  attr_accessor :name
  @@all =[]
    
  def initialize(name)
    @name = name
    @@all << self
  end
    
  def statement
    puts “The name of this song is #{name}.”
  end
    
  def self.create(song)
    song = Song.new(name)
    song.save
    song
  end
    
  def self.all
    @@all
  end
    
  def save
    self.class.all << self
  end


end
```


This is how we instantiate or create an instance of the object(sports car) from nothing or out of no where.

*Figure 1.2.*
```
song = Song.new("In an Aeroplane Over the Sea"). 
```
*song* is just the name we choose for the instance. The important part is the *Song.new* *Song* is the class and *.new* is the instantiation. 

Back in the Class definition in *Figure 1.2* The first method is an initialize method, which is a kind of instance method but it doesn’t need to be called on the object like an instance method does. It automatically comes with every instance of the Song class as a parameter. So when we create an object song like below the parameter "In an Aeroplane Over the Sea" takes up the name attribute and becomes the name of the song. 

*Figure 1.3*
```
song = Song.new("In an Aeroplane Over the Sea")
song.statement
```
Above in *Figure 1.3* the *statement* instance method(bicycle couriers) is called on the song instance. They deliver a message to the instance method. In this case the message is "The name of this song is In an Aeroplane Over the Sea." If you are having trouble understanding this look back at *Figure 1* in the *def* *statement* you can see where the name variable is included in the statement.

Finally class methods only get called directly on the class of the object as opposed to instance objects that get called on the instance. These taxi drivers can't be called outside of the city the city being the class. So they are called like so:

*Figure 2*
```
Song.all
```
In *Figure 2* the "Song" is the class. Usually class names are capitalized so that it is easy to recognize it from an instance method. Unlike in Figure 1.3 where the instance method name "song" is in lower case.

And that is all there is to it. 




