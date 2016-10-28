---
layout: post
title:  "NewsMap - A Rails Web App for people to create and share news "
date:   2016-10-28 06:02:12 -0400
---



I've always had a zealous imagination and a desire to create and build things . So, It didn't take long before I started drawing my thoughts on paper. I wanted to use my imagination to create things that others could gaze upon and marvel. But that soon evolved and eventually transformed into a desire to create something more tangible something that people could interact with and use as a tool. That is where programming came in.



**My intangible Idea**

Today the news is owned by roughly 6 or so large companies like Comcast, Viacom, Time Warner etc.. that have sought of a monopoly over most of the broadcasting agents like CNN, Fox news to name a few. I've read that over 90% of the news are owned and probably influenced by these companies. Makes you question the legitimacy of the news. 

Now wouldn't it be great if that control was placed in the hands of people like you and I . Imagine a Network of People all around the world documenting their experiences on an App that is opened to everyone. Call it "Social News" better - call it "NewsMap."
This is when I directed my thoughts into execution, and used the tools I gathered from programming. 



**Turning My Intangible Idea into to something Tangible **

The language of choice - Ruby. The Framework - Rails!

Rails is an engine with many goodies built in. 

A few details worth mentioning: If you are new to rails like everything else in programming when there is a new update it is important to look over the changes. I had a painful reminder of this while building a form that users could enter information for creating new articles.

While testing the form to see if it would persist the data,  every attempt to save the data to the database would fail. The article object or instance would “rollback" (this happens when data tries to save but a protocol prevents it from doing so).  

Un·be·known to me Rails' most recent updated to version 5 barred all associations to now require as default an associated value for any *belongs to relationship*. And If there is none you will get an error!

What this means for example is:

```
If user has_many articles   articles_must_be_together_with_user_at_all_times in holy matrimony forever
```

Or at least until you tell the two classes that they don't need to be together. But how do we do this in Rails?

Let's start with the error I was getting. It was on an article that I architectured to be associated with to User class. The relayed error wasn't very helpful though, so to get more details form the error I called *errors.fullmessages* on the part of the code I suspected was causing the error like so:

```
post.errors.full_messages.to_sentence

"requires a user"

```

That was the error "Requires a user." Easy to read, right? This was one of the intermediary barriers I had when building the app. 

For consistency, here is a list of the the protocols that I was reminded of during this project.

1. Use Strong Params for forms and nested forms
2. Sanitize your params
3. Make the methods that you don't use outside of the controller or model private to save memory
4. User partials where appropiate. Example (forms, repetitive code, Navigation).
5. User helper methods
6. Create custom helper methods for easy access.
7. Create nested routes so other users can use a url's that simply makes more sense.
8. Separation of concerns: less or "0" logic in the views. Define the functions within the controllers to match the model and views they are based on
9.  (Skinny Controllers, Fat Models) Most of the logic goes in the model as it is closer to the database. 




**How the Web App works**

But enough about the technicals. Let’s talk about the app and it's features. 

Options users have:

1. Find an Article by Category:
2. Find an Article by Country:

Features at play:

1. Associations (relations between objects) "users" + "articles" + "sources" + "validations" + "categories" + "countries"
2. Sessions "login" or "logout"
3. User roles: "reader" or "author"

These features are at the core of thsi app and are what distinguishes certain users from other's and their functions.
Because it is essentially a news + networking app I wanted there to be a clear division for the roles of the users. 
With this I created two roles: 

*An author* - Creates articles for readers or guest to view 

*A reader* -  Can validate any article he or she think is legitimate or worthy of merit.

What this means is when a person signs up they have a choice to be a reader or an author. When signed in the reader can click on any article they wish to read from any country or category of choice. If they think that the article was legitimate for whatever reason they can click a "validate button" which will show as a number next to the autthor's article.
So, if 3 readers clicked the button then that article will have three validations next to it. 
An additional feature that i did not include could be an option to click a "not valid" button but that is yet to be decided.

To avoid readers from validating the same article more than once the reader is prompted by a notification that tells them "You already validated this article." To prevent any subjectivity or competition Authors are not allowed to validate other authors' articles. There is also a feature on the main page that shows the article with the most validations among all authors.

Another useful feature that I built is waht Ruby on Rails calls a *custom attribute writer* via *nested forms* . This allows the author the option to add their own source to their article while they are creating it. Authors could also choose from checkbox options ranging from independent to nonprofit news agencies, none of which are owned by the aforementioned, big companies.  
Additionaly authors can delete their forms or edit them as they see fit and readers can only validate and read author's articles. 

Lastly if you're wondering why I chose the name News Map, it will have a full screen google map like interface of the globe where users can click on any part of the world and read news that's happening in that region. 

A solution for sharing news and knowledge among fellow people of the world.

