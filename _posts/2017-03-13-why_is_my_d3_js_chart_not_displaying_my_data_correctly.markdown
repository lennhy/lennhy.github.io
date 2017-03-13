---
layout: post
title:  "Why is my d3.js chart not displaying my data correctly?"
date:   2017-03-13 00:07:44 -0400
---

FYI: This is for folks who are already familar with d3.js but have encounterred the fustrating problem with there data plotting incorrectly. 
After wokring on web applications for some time. I recently had the time to venture into data visualization. I've always wanted to explore this realm given my background in Electronic Design and Multimedia. As a designer we learn to display information that is beautiful, minimal and easy to understand but not interactive by a user. That part is left up to the developers. Now that I am a developer I am able to turn my designs into an interactive playground that is both dynamic and interactive. Something that could not be done only with design. 

My project was to make an interactive weather chart where the user could hover over dots that marked every change in temperature throughout the 7 day week startinng with "today". There are several apis and frameworks one can use such as : HTML5 svg and canvas for instance. Given it's popularity amongst tech companys and developers I decided to go with d3.js.

According to the website D3.js is a JavaScript library for manipulating documents based on data. D3 helps you bring data to life using HTML, SVG, and CSS. D3’s emphasis on web standards gives you the full capabilities of modern browsers without tying yourself to a proprietary framework. What this means is that it allows you to write code that is no different from writting code without the framework. It's the same tools (svg, canvas, css) that we would normally used to draw graphics in a website that is used in d3.js.

At first the docs can seem intimidating but once you get to the part that you need applying the code seems simple. In my case I needed to draw a multichart graph so I watched a lynda.com [tutorial](https://www.lynda.com/D3js-tutorials/Data-Visualization-D3js/162449-2.html/) on d3.js. I also read some of the docs on d3.js [github-wiki](https://github.com/d3/d3/wiki) page. 

It's simple, you have to download the js file for d3. To organize my files I used bower.js and saved the framework iin my project. The following are instructions for how to use bower.js [Here](https://github.com/bower/bower)

Enter the necessary js links in your index.html page.

To area for the chart to actually show up in the DOM you have to include this script in a js file and link it to your index.html file as well. 

```
d3.select("element")
```

The important part is the d3.select. This selects any element you put on your page such as a div for instance. 

Now to the meat of the story. Since I created a multiline chart I needed x and y coordinates to plot the line. 
In the DOM the points are plotted from left to right for the x-axis and top to bottom for the y-axis.

So to draw a straight line you would write x = 0 y = 0 for the starting point, and for the ending point x = 100 y = -100. 
Easy enough right?

I had to plot a total of 0 - 100 on the y-axis and 1 - 7 (days) on the x-axis
It turns out whenever you are plotting the lines on the y-axis you with positive values you must minus the positve values from 100. This is the only way  the values will show up as positive values. Because remember the y-axis goes down, so by default it will plot the points downwalrd from 0 to -100 and not updwards from 0 to 100. 

So if the values were 10, 20, 44, 55, I would have to minus 100 from 10 = 90, 100 - 20 = 80, 100 - 44 = 56, 100 - 55 = 45 to get teh correct values so that the values do not appear inverted. So 10 would plot on the grpah 90 pixels down which is equal to 10 pixels upward. Get it? That's positive 10 on the chart!

Here is the part of the code where I minus the values (High and Lows) from 100.

```
  var lows = weatherData.map((e,i)=>{  return 100 - Number(e.low);})
        var highs = weatherData.map((e,i)=>{  return 100 - Number(e.high);})
```

Then poltted them onto the graph with the d3.js function for a line d3.line/

```
 line = d3.line()
            .x(function(d, i){;return d.x*60;})
            .y(function(d, i){return d.y*5;})
            .curve(d3.curveCardinal);

       //  Create d3 line generator
       //  highs
       lineTwo = d3.line()
            .x(function(d, i){return d.x*60;})
            .y(function(d, i){return d.y*5;})
            .curve(d3.curveCardinal);
```

You can see my chart live [here](https://rocky-garden-18908.herokuapp.com/)



