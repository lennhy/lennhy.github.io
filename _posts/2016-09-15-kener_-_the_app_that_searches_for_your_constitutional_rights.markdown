---
layout: post
title:  "Kener - The app that searches for your constitutional rights. "
date:   2016-09-15 03:02:48 -0400
---


First off why did I chose the name *Kener*? "Ken": To know, knowledge. "er": a person or thing that performs a specified action: reader, decanter, lighter...
Hence the name *Kener*.

While building this application I quickly discovered that the work I needed to put in would be tremendous. If I were to compare software development to another trade, I would compare it to being that of a doctor who does not have a specialty but can work in any field. Whether it be emergency medicine or heart surgery that doctor can do it. The same goes for a software developer. It's no wonder why people have created so many different names to address the role of a software developer. Few that come to mind are: Software Engineer, Web Developer, Engineer, Developer, Computer Scientist... However varied, they all share one thing in common, they require a large investment of your time into study and practice.

Which leads me to this point that to pursue a career in Software Development one doesn't have to invest loads of money for an education in Computer Science. The one requirement is that you invest the necessary time and have the unwavering quality of persistence to learn and practice programming the right way until you get better. It's like that saying "practice practice" or "don't give up..."

With that said here are the important points you should have in mind that will help lead you to the right track to starting a successful project or career as a software developer.
 
**Be prepared to constantly read through tons of documentation, which never ends.**

Because of the nature of software development things are ever changing. We constantly find new and improved ways to build things and to make them better. This is a key quality of software engineering and it is why technology has evolved from analogue devices to the complex digital and virtual machines of our present. In my time "beating the code" one of the things that I've realized is that once you start learning programming the learning never ends. And this isn't only because things are constantly changing. As humans we tend to forget things and can absorb only a small percentage of what we learn. Luckily the principle concepts of what we learn are much easier to stay with us for a longer time. By this I mean the ubiquitous processes and practices we implement into our code. Like design patterns such as **DRY** (Don't repeat yourself) or **Reusable code**.

These are the things I learn while working on my Sinatra Application. Sinatra is the name for a web frame work built on top of Ruby. The name was inspired from Frank Sinatra. Apparently the programmers are big fans of the icon. 

Another important ideology I learnt is that essentially most applications in one form or another is a CMS or Content Management System. So if you can successfully build one then you are off to a good start as a programmer. 

With my Sinatra Application I wanted to build something meaningful, more specifically something that could be useful to the underrepresented groups, minorities, marginalized communities where knowledge of one's constitutional rights are not inherently taught and where without this knowledge one could be exploited by those in authority. And by authority I don't just mean the police, I also mean your boss, your landlord, your professor anyone who has authority over you in some way . Although, I was inspired to create this app after seing the numerous acts of violence by certain police officers in the US, which could have possibly been avoided or quelled had not the victims have prior knowledge of their rights.

Although this project may seem ambitious for my first CMS it is still in its early stages. It does however perform the basic job of creating a search option, where the user can type a key word. The key word is then sent to the database which returns an amendment or amendments that closely match the key word. The question is then posted on the main page of the application alongside questions belonging to other registered users.

Users can also create a user account and save their questions and associating amendment on their user page. That way they can remember it without having to repeat their search.  Because it is a CMS the user can also edit or delete their question. When the question is deleted it is automatically removed from the main page and from their user page. If they decide to edit the question, a matching amendment result is returned. 
You can see how to use the app here: [Instructional video](https://vimeo.com/182806199)

Just imagine the scenarios where this can be helpful. Most importantly how it can be applied is key. Imagine a situation where the user needs to defend himself by spitting some knowledge on a an authority figure whether it be an overzealous police officer or a disgruntled security guard; being able to type a quick word into the app will give you a tremendous advantage where ignorance could otherwise be a point of exploitation. 

Since the App was built in Sinatra it is a web based application but with the addition of the bootstrap framework this will give it a responsive design so users can view and use it on their phones and other mobile devices. Ideally In the future I would want to create a mobile app version for android and iphone as well as allow users to comment and add to user's posted questions. Ultimately My goal is to empower people and to create an environment for learning and gaining knowledge. 

In addition to the SInatra framework there were important concepts incorperated into the making of *Kener*. First is 
*Restful Routes* . A key concept in this application, Restful routes allow the application’s url to change dynamically without using fixed end points that the user can just type into the url. Instead the program does this for you. Every time you click a submit button, or a link restful routes are at work behind the scenes directing the user to the correct path. For example, if you try to sign up and the information you entered is not valid like a incorrect email format the application will be send you back to the sign up page. Or if you logout the application will send you to the login page instead of the signup page. A better example is if a user tries to edit or delete another user's posted question he will be sent back to main page because he is not allowed to change another user's question. These actions are all happening through a system of routes and are controlled by something we call a Controller. A Controller sets up the business logic of you application or assigns the functions for every page.

Inside the controller are actions that include methods like http requests. These request comprise mostly GET, POST, PATCH, DELETE which are part of a system we programmers call CRUD (Create, Read, Update, Delete). These controller actions provide resources for the Server to act on and influence what we see in the client side of the application, such as our posts, username and general content. 

In each controller action there are finder methods that send data to the database to retrieve specific information that we can view on the html page of the app. The data is then transposed into html in the views folder of the application (where all the html files are stored) via these erb tags (<%= %>). What goes between them is the Ruby code from the variables that are created in the controller.  This is how Websites like Facebook can have millions of pages for its users, by creating one view page called "layout". This main page contains elements that never change such as the navigation bar and footer. With the erb tags it yields the pages that can display varying data so the header, navigation, footer don't have to be repeated in each page view. See the example below:

example: <%= yield %>

With Sinatra I was able to quickly create ORMS (object Relational Mapping) with the database tables designing a belongs to and has many relationships among 3 classes. The Question class, the Amendment class and the User class. With these names anyone can guess their functions. These classes represent the application's real life models. The User model is a user in the real world who is logged into the application. The Question is the question you type into the search field and the Amendment is the amendment that is returned as a result of the search from the database. Such is the power of ORM's. Also with Active Record (A database framework that allows for easy and automatic metaprogramming and built in methods) I can build on the data and form relationships among the models. A User has many Questions and many Amendments through Questions, Questions have many answers or in this case Amendments and Amendments belong to Questions. I know it sounds confusing but what this is simply saying is that each model is related to one another in some way and can share data based on their relationship. It's the same as saying if I asked someone a question then that question belongs to me because it came from me. But as we know other people can ask the same question so in turn that particular question can belong to many different people. 

For a list of the main components and concepts of this Sinatra Application that I may or may not have left out see below:

**Components:**
    Sinatra - is a free and open source software web application library and domain-specific language written in Ruby
		Rack - A ruby webserver interface. Creates an environment for database management and server.  
		Shotgun - A virtual server
		Active Record - 
		SQL - A Database Language
		Ruby - The Programing Language of Choice
		HTML - a markup language for describing web documents  
 
**Concepts:**
    Restful Routes - provides a mapping between HTTP verbs, controller actions, and CRUD operations in a database.
		ORM - Converts data type models into tables in a database which share data and relate to each other through the data    that it shares.
		Controller - Controls the business logic. 
		ERB - B (Embedded Ruby) enables you to generate any kind of text, in any quantity, from templates. 
		Templates - create different views from one template with the use of ERB
		Inheritance - When child classes inherit methods from Parent classes.

