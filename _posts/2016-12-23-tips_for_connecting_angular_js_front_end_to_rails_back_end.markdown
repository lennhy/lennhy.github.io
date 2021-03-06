---
layout: post
title:  "Tips for Connecting Angular JS Front End to Rails Back End "
date:   2016-12-22 20:33:20 -0500
---


Picture Angualr JS like a tool box that we can use to manipulate our car engine (the Rails backend api) and our toolbox comes with tools that we call services.


ANGULAR TOOLS

   1. Services
   2. http
   3. UI-Router
   4. Promises
   3. Forms
   4. Controllers
   5. x-CSRF Token
   

RAILS

   1. Controller
   2. Routes
   3. Serializer
   4. Bower
	




Intro =====

When starting a project where you are connecting a front-end framework like Angular with the rails API It will save you a lot of time if you use the package manager *Bower* to download all your front end dependencies and help create the correct structure for your asset pipeline. If done correctly this will resolve any issues like rails not knowing where your HTML templates are in your  JavaScript folder in relation to your rails views/layouts/application.html.erb and views/application/index.html.erb templates. These rails templates hold the main layout where all your HTML templates from your javascript folder will be placed.




The asset pipline =====

The key to keeping the correct file and folder structure so that rails knows where all you view templates for angular are in relation to your main layout file is to maintain your appliication.js with the following required dependencies:

```
//= require jquery
//= require jquery_ujs
//= require angular
//= require angular-ui-router
//= require angular-rails-templates
//= require moment
//= require_tree .

```

An example of a problem I ran into by not including one of the above dependencies was that when I clicked the sign-out link, instead of the delete request  to remove my current session token, rails would perform a get request for the sign-out link causing a route error . This problem was solved by including the require jquery_ujs. 




Angular Forms and persisting or changing data =====


In Angular + Rails instead of writing out the action and method directly in the form like we would in a Rails app we create a function that creates , updates, destroy etc... the data which is usually executed via ng-submit or the ng-click directive. 

When you are creating a has_many relationship for example I made a comic model that has many genres so a comic would have a column that housed an array of genre_ids. In order to save several genres to a comic I created checkboxes for each genre.

But sometimes when you create a check-box form with angular's ng-repeat you can run into problems like returning true or false instead of the value you want or all the select boxes being checked when you only selected one. 
My solution for this was to use $index as a value in the ng-model that is adding the data to the array of genre_ids. Look below for the answer:


```
  <span ng-repeat=" genre in vm.genres">

    <label for="{{genre.id}}">{{genre.name}}</label>

    <input ng-model="vm.book.genre_ids[$index]"  type= "checkbox" ng-true-value="{{genre.id}}" ng-false-value="null"/>

 </span>
```



Routing  =====
 
Use UI-Router. You can use the resolve key in the state method to return and display any data before the routes for any get or post requests are loaded . Then the data on the DOM will be updated without any flicker or noticeable change. However sometimes when you try to manipulate the any possible data this way it might return undefined. When this happens it means that the success promise has not been fullfilled yet as a result of the Asynchronous Javascript or AJAX call. In this case what we need to do is manipulate the data in the .then or .success callback for the promise which will reuturn the data we want to manipulate. We can do this in our Service instead of the js routes under resolve. 




Persist data  =====

Use this in your app.js file to avoid any possible CSRF errors with form submissions:           $httpProvider.defaults.headers.common['X-CSRF-Token'] = $('meta[name=csrf-token]').attr('content');

In your Rails API Make sure you build a controller, action and corresponding route for every model that you serialize with active_model_serializer. This gives you the correct url to use for your service to process the json data after it is resolved by the UI-Router . With this data the service can then return  all the data associated with the comic model without reloading the page. For example to return a json object url for all my comics this is what I did:

*a serializer for my comic model:*

```
  attributes :id, :title, :description, :issue, :volume, :page_count, :issue_date, :graphic_novel, :region_id, :genre_id
  has_many :users, serializer: UserSerializer
  has_many :genres, serializer: GenreSerializer
  has_one :region
```

*The Comics controller:*

```
  def index
    render json: Comic.all
  end

```

*A route in my config/routes.rb file like so:*


```
  get '/comics' => 'comics#index'
```
 
 
	 
