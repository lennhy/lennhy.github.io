---
layout: post
title:  "Making a simple custom search filter with JavaScript and SQL"
date:   2017-01-16 17:00:42 -0500
---


Ever wondered how webpages implement filters ? That little search box where you can type what ever you want to search for and the information on the web page updates like just magic. On every keystroke it dynamically updates the DOM changing the content literally from your finger tips. 

This kind of functionality has been around since the first search engine so there are libraries of filters that you could implement into your code with actually doing any coding.

But we don't want this we want to be able to write our own custom filters to stand amongst the programmers that can create from their thoughts and not use someone else's code. To go a step further We don't want to use libraries like JQuery, or  database library like Active Record to make it easier, no - we want to use native JavaScript and SQL. In fact we won't even use the JavaScript filter method! It just got real. 

Why? Because if you want to have more than just a basic understanding of any particular language like JavaScript or SQL, you need to know how to use it. You need to know how to implement patterns and algorithms in that language and not rely on framworks or libraries to do your dirty work!

For this tutorial I created a custom filter through trial and error using Javascrit, SQL and Ruby. 
My web app: returns amendments from the constituion and saves it the user's acccount. 

At first I wanted to create a search function similar to Google's search where the user can type in a sentence in a search input to return all relevant information using the wildcards. To create a custom search in SQL you use the [wildcard ](http://www.w3schools.com/sql/sql_wildcards.asp) characters with the word "LIKE" (SQL wildcards are used to search for data within a table). See below.

```
SELECT * FROM Customers
WHERE City LIKE 'ber%';
```

But this approach is limited. Since each SQL statement can only query one item at a time users can only search by a single word or combinations of words per SQL statement. For example the 'ber%' above would be the search text entered by the user and the Select statement would look in the database table for anything with that word. 
I did htis below with my SQL code in Ruby. 

```
     ("SELECT * FROM Amendments WHERE " %#{word}%
			 @amendment = Amendment.find_by_sql("SELECT * FROM Name LIKE '%#{word}%' OR Content LIKE '%#{word}%')"
```


However in most web search inputs the search algorythm does not necessarily work like this. Usually the user would never have to type a whoie sentence before the DOM returns or updates the relevant data. When the user types a word into the text field  the SQL will retrieve the data and update the DOM immediately with the relevant information.

Have you ever noticed that when you type into a search box more than one word if the words do not exactly match the name of item in question then the result will return nothing? That's because it tries to match the word or combination of words exactly with the content in the table. So typing in a sentence like "I want to see Star Wars the Force Awakens" in a Fandango website search box will return 'NO RESULTS FOR "I WANT TO SEE THE FORCE AWAKENS"'.  

So to make the SQL ssearch more intuitive for the user I used JavaScript to create a simple algorythm that updates the DOM everytime the user enters a letter or string of letters in the input. This inturn gives direct feedback to the user so they know what their search is returning. Here is the JavaScript code below.

*code.js file
*
```
var liNodes = document.querySelectorAll('ol.amendments li');
var inputText = document.getElementById("searchBox");


function Amendments(elementNodes, searchText) {
  this.textNodes = elementNodes;
  this.searchText = searchText.toLowerCase();
}

Amendments.prototype.filterAmendments = function(){
  let regex = new RegExp(this.searchText);

  let list = document.getElementsByClassName('amendments');

  let result = [];

  for(let i=0; i < this.textNodes.length; i++){
    let textNode = this.textNodes[i].innerHTML;
    let cleanTextNode = textNode.replace(/;/g, " ").toLowerCase();

    if(regex.test(cleanTextNode)){
       result.push("<li>"+ textNode + "</li>");
    }
  }
  list[0].innerHTML = result;
}

// if there is something to return start the eventListener
window.onload = function runCode(){
  if (inputText !== null) {
    // Dynamically update the DOM or console every time user enters a key
    inputText.addEventListener('keyup', function() {
      // save value that user types in search box
      var search = inputText.value;
      // create a new instance of the amendments constructor
      var filteredAmendments = new Amendments(liNodes, search);
       filteredAmendments.filterAmendments();
    }, false);
  }
}

```

In the first 2 lines I saved the search input and the div attribute which holds the list element in two variables.

```
var liNodes = document.querySelectorAll('ol.amendments li');
var inputText = document.getElementById("searchBox");

```

In the next line I created a constructor that holds the search input and the list element variables as arguments.

```

function Amendments(elementNodes, searchText) {
  this.textNodes = elementNodes;
  this.searchText = searchText.toLowerCase();
}

```

Then I created a method on the constructor. This method will loop through each amendment in the DOM and filter the content that matches a regex Expression. 

*let regex* is the regex object 
let list is array and is the the list that holds all the amendments in the DOM.

```
  let regex = new RegExp(this.searchText);
```

Each list element is nested inside the list array varaible and each element has attributes nested inside of it. 

```
  let list = document.getElementsByClassName('amendments');

```


The element I need is the innerHTML.
After removing semicolons to prevent JavaScript Injection 

```
    let cleanTextNode = textNode.replace(/;/g, " ").toLowerCase();

```

I saved the innerHTML of each list as *textNode*  and matched the regex Expression with *textNode* variable if the regex matched then each textNode would be pushed into the result array and returned to the DOM after the loop was ended. 

```
if(regex.test(cleanTextNode)){
       result.push("<li>"+ textNode + "</li>");
```

The code in the Window.onload runCode function works like an "On" button. It is responsible for calling all the code including registering the user input on every key stroke and calling the filterAmendments method.

I also iterated over all the amendments in the database so Instead of grabbing a specific amendment in the database with SQL when the DOM loads ...

*create_questions.erb file
*
```
<ol class = "amendments">
    <% @amendments.each_with_index do |a, i| %>
      <li><%= "#{a.name}: " + a.content %></li><br>
    <% end %>
  </ol>
```

ruby will  grab all the content from the database and spit it onto the DOM. 

While the Javascript will filter the data.

So now when the user enters a key into the search box the DOM will dynamilcally update with the relevant information via this Javascript program. 

The SQL is only executed after the user enters the word in the seach box and clicks on the save submit button which will return the specific amendment or in this project will save it to the users account. 

*Questions Controller
*
```
if @amendment.nil? || @amendment == ""
        redirect to "/users_questions/create_question"
      else
      # -- associate the amendment that was returned to belong to the question
        @question.amendments << @amendment
        @question.save
```


