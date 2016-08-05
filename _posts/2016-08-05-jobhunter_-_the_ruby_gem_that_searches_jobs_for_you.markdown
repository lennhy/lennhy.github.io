---
layout: post
title:  "JobHunter  - the Ruby Gem that searches jobs for you"
date:   2016-08-05 02:05:41 -0400
---


Ruby gems are snippets of code that are built to be used as a plugin with a particular function and is packaged in such a way that it can be used by anyone and easily modified. There are gems built for anything you can't think of and chances are if you imagined it, it has probably been built before. 

With this in mind I decided to build a Ruby gem that addresses an immediate issue I had and given the recent 84 downloads I've seen within 2 hours of publishing my gem on rubygems.org others like myself share as well. This issue was to find a job that accurately matched my skills and experience and didn't require as much time scouring the internet to find.

To do this I would have to find a single page website that I could scrape the information off of and return it in a human readable format. For those of you who don't know what scraping is - it's a way to get information from web pages via their html or xml files and use them to get any information you want. For instance, one could go on a local newspaper's website and scrape all the articles about crime and get a node-set detailing all the locations where there was criminal activity. Then use this to draw a pattern showing the location where crime is most prevalent. 

In order to accomplish my task of scraping jobs from the internet I went to one of the more popular jobsites on the internet: [Indeed.com](http://). Housing millions of jobs world-wide *Indeed* is a great resource for finding jobs. However,  there was one problem - *Indeed* is not a single page website. In order to find any particular job, the user must enter a custom search in a search box that will return the jobs listing based on the search. That would mean I would have to scrape every page that loaded from the results of the search making scraping much more difficult or I would have to use an additional program to help streamline the task. 

I looked at other job sites that were built in a single page structure like [garysguide](http://).com and [angellist.co](http://) but were not as vast as Indeed neither were they plentiful. Then I searched for API's (Application program interface) at this great resource [http://www.programmableweb.com/category/all/apis](http://) , (It claims to be the largest API's directory on the web). and I found that *Indeed* has an API. For reference what an API does is simply a library of code similar to a Ruby gem that helps you build other programs. In this way it would help me build my Ruby gem.


The API works like this: It provides you with an XML feed which uses an http request that has key words / letters that equal to a value that you can change to customize your search.

Below is an example of what it looks like.

The long line of text is the XML request. In it are key words like my publisher's ID q for query (enter any keyword search) l for location of the job for example "New York" a limit for how many jobs you want  returned after you return a search ect. etc. You get the idea. The data that was returned was in XLM format which has its own markup. For example, If I did a search with a location of New York it would return a location header that would read "New York."

Like so: "<location> New York <location>" Pretty neat! You could even use the http link and paste in the browser's url window and it would return the entire xml document. But this would not be very useful if we wanted other folks from around the world to use it and to read it in a legible and concise format. We would then have to parse it with a program. One such program is Nokogiri a ruby gem that searches and parses documents from the web returning a node-set of elements. 

Now the question that is left is how do I get the results to return in a human readable format? To do this we would install the "Nokogiri" gem and require Ruby's built in module "Open uri. " Open uri is a sought of surrogate or a wrapper for Nokogiri's method call to parse data. 

See below for an example:

    `doc = Nokogiri::XML(open(uri))`

Here Nokogiri is making a XLM request and opening it with the open uroi wrapper. Inside the wrapper lies the url variable that is an argument for *Indeed's* http XML request link. 

Now on to the tricky part. So I had my code set up to scrape the information now I needed a way to 1. be able to change the link based on user input and 2. be able to create an object that would return the information in a readable format. Introduce the Command Line Interface!

See below:

<a href="http://imgur.com/UjfSTPq"><img src="http://i.imgur.com/UjfSTPq.png" title="source: imgur.com" /></a>
*Figure 1.*

The command line is an easy way for developers to relay readable information to users .
Since the Command Line - CLI for short is where the user starts interacting with the data I started conceptualizing my project from this standpoint. So I told myself that I would have to make a *Scraper Class* that would scrape the information from the XML request with *Nokogiri* and a CLI Class for the command line to display and get information from the user that would be transmitted into the Scrape Class to begin the initial scraping But It would be nice to return the information as a more concise object, modelled after a Job in the real world. So I created a *Job Class.* The purpose of the *Job Class* is to create *Instance Variables* that contained the key words / letters from the indeed API's http XML request which I mentioned earlier (example "<location> New York </location>") and save it as methods in the Job Class which I could call on later to return the values. Such values would be the location, time of which the job was posted and the url to apply for the job on indeed all specific information for the user to get once he inputs his customize search. 

Here is a snippet of the instance variables I created in the Job Class or what is called att_accessors. 

See below:

  `attr_accessor :job_role, :company, :city, :state, :country, :url, :description, :date_posted, :post_duration`

Now all I had to do was to build the logic that allowed the user to type in the job query, location, country, limit of results and distance within the location that would be saved to the Scraper class. To do this I built if else conditional statements, set an input to prompt the user to enter information which would then be saved as instance variables in the Scraper class once I called them as methods on an instance of the Scraper class like so: `scraper.co` to save the country (co). 

See below for more details:

```
puts "1. Enter the max number of the search results you want return"
      if input && input!="exit"
        input = gets.strip
        scraper.limit = "limit=" + input
      end

      puts "2. Enter any query or name of job role"
      if input && input!="exit"
      input = gets.strip
          scraper.q = "q=" + input
      end

      puts "3. Enter initals or name of country"
      if input && input!="exit"
      input = gets.strip
        scraper.co = "co=" + input
      end

      puts "4. Enter a postal code or a city"
      if input && input!="exit"
        input = gets.strip
        scraper.l = "l=" + input
      end

      puts "5. Enter a number for distance from search location"
      if input && input!="exit"
        input = gets.strip
        scraper.radius = "radius=" + input
      end
```
The code above would then be saved into the scraper class as instance variables in the http request and return a customized list of jobs and job details. See below for how I saved them inside the http link.

   ` uri = "http://api.indeed.com/ads/apisearch?publisher=3881286689960538&#{@q}&#{@l}&sort=&#{@radius}&st=&jt=&start=&#{@limit}&fromage=&filter=&latlong=1&#{@co}&chnl=&userip=1.2.3.4&useragent=Mozilla/%2F4.0%28Firefox%29&v=2"`

In  the code snippet above the words / letters with the @ symbol attached are the instance variables that save the data from the CLI input.

Now I wanted to display this information with the Job class methods but unfortunately I encountered some problems getting the instance of a job to collaborate with the scraper class. So I decided to iterate over the array that the Scraper instance method scrape jobs returned and print the information out onto the CLI and this worked like a charm!

See the code below.
```
if input !="exit"
        scraper.scrape_jobs  # start scrape
        jobs_array = scraper.scrape_jobs  # return the array of hash objects from scrape
        counter=0 # apply numbering to results
        jobs_array.each do |a|
          puts "|" + "#{counter+=1}" +"|"
          a.each do |key,value|
            puts "#{key.capitalize}: #{value}"
          end # end of second each enumerable
          puts ""
        end # end of first each enumerable
      else
        puts "Exit"
      end # end of if statement
    end  # end of until statement

```
And wallah! However, this wasn't the end of it. With making a gem comes a particular domain structure which I had to painfully learn through trial and error. I learnt that there are many ways to build a gem once you are finished creating the logic. But once I settled on the *bundle gem* and *gem build* command I was able to create a gemspec file
with a list of all the gem's dependencies and from that create the gem. 

Returning to what I said about the domain structure, the most important thing is to have the executable file in the right place and to have your gemspec file direct to this path. As this is how your computer's operating system knows to run the gem once it is installed - by simply typing the name of the gem in the command line. To change the path you would have to go into the gemspec file and change the *spec.bindir*  spec, a fact that I didn't know and what caused me to yank a gem that I had already publish, rebuild my gem and republish to rubygems.org under another version. version 0.1.1. As the first version 0.1.0 did not work. This was because my executable folder was not saved as 'bin' but instead it was saved as 'exe.' So when I installed the gem locally on my computer every time I type the name of the gem (job_hunter_cli) nothing would happen. The computer was looking for a folder called 'exe' and not 'bin.' So I changed the path to 'bin' and it worked.

After several attempts I was finally able to publish my gem here at [https://rubygems.org/gems/job_hunter_cli](http://).
Although not perfect the gem does what I intended, able to customize the result based on the user input. Here is a quick instructional video on how to use it. [https://vimeo.com/177637445](http://)

