---
layout: post
title:  "How to Deploy a Sinatra Rack Based Application to Heroku  + GitHub sync"
date:   2017-03-19 20:31:41 -0400
---


This tutorial assumes that you have already created a Sinatra Application and that you are using the sqlite gem for development. It also assumes that you have already signed up for a heroku account. 
To make it simple we will focus on the free tier heroku dyno types. 

**Important points to remember for deploying to Heroku.**

1. You must include these in your gemfile for your app to run in the Heroku servers:
    ruby version in your gem file
    puma gem 
    Group your sqlite in the Development group, pg and  activerecord-postgresql-adapter in the Production group.
2. Include a Procfile that goes in the config folder
3. Include a database.yml file that goes in the config folder
4. The heroku command "heroku logs" is your friend. Use it for debugging errors that you may encounter when launching your app. it's the closest thing you have to pry as heroku does not use this gem in produciton. 
5. SQlite does not work on Heroku production environment.


First of all if you were having dificulties before reading this don't get discouraged, I spent some time pulling out my hair tyring to set up the environment to deploy the application. It took me longer because their was not a lot of tutorials available for this specific case (Sinatra+Heroku+SQlite). Also the heroku, postgres and sinatra docs were not tailoired for something this specific. So I used all theses resources including other student blogs to solve the issues I faced but not without some challenge. 

**SINATRA SECTION**

**Setting up your Sinatra Environment** 

Copy this code in your environment file. I will explain this shortly

*config/environment*
```
require 'bundler'
Bundler.require(:default, :development, :production)
ENV['SINATRA_ENV'] ||= "development"

configure :development do
  ActiveRecord::Base.establish_connection(
    :adapter => "sqlite3",
    :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
  )
end


configure :production do
  db =  URI.parse(ENV['DATABASE_URL'] || 'postgres_database_url')
#  db =  'postgres://localhost/topofthemorning_development'

  ActiveRecord::Base.establish_connection(
    :adapter  => db.scheme == 'postgres' ? 'postgresql' : db.scheme,
    :host     => db.host,
    :username => db.user,
    :password => db.password,
    :database => db.path[1..-1],
    :encoding => 'UTF8'

  )
end

require_relative '../app/controllers/application_controller.rb'
require_all 'app'
set :public_folder, File.dirname(__FILE__) + '/public'

```

The first part iis where you set your sintara environment to wahtever Heroku changes it to (production) or it will default to development.  

The second part "configure development" is where you set the development enviroment to use  sqlite and sqilite's database that is on your computer

The third part  "configure produciton" is where you set the  production environment to use postgres databse for production. After the || symbol you will enter the postgres databse url. You can get this url from your heroku account [here](https://data.heroku.com/) Click on the database link and on that page you will see a section that says database credentials. Click on "view credentials"

**Create a database.yml file **
```
development:
  adapter: sqlite3
  database: database.db

test:
  adapter: sqlite3
  database: database.db

production:
  adapter: postgresql
  host: localhost
  url: <%= ENV["DATABASE_URL"] %>
```

This will tell the application what environments to use for each database.

**HEROKU SECTION**

**Create a Procfile**
You need this file so that Heroku knows how to startup the application when it is launched. Sinatra is a Rack based applcation so type the code below.

```
web: bundle exec puma -p $PORT -e $RACK_ENV -t 0:5

```


**Step 1**
In your terminal type 

```
heroku login
```

Then enter your heroku account email and paassword credentials 

**Step 2 **

If you have not already added your project to be tracked by git cd into your application:

```
cd myapp
 git init
Initialized empty Git repository in .git/
$ git add .
$ git commit -m "my first commit"
```

If you already tracking your local repo with git then skip to step 3 

**Step 3**

Create a new Heroku application

```
 heroku create
Creating falling-wind-1624... done, stack is cedar-14
http://falling-wind-1624.herokuapp.com/ | https://git.heroku.com/falling-wind-1624.git
Git remote heroku added
```

If you don't already have a github remote for the applcaition on your local computer and you want to use Heroku's server to push code to instead of Github's then type the code below to setup a remote server with Heroku 

```
 heroku git:remote -a falling-wind-1624
Git remote heroku added.
```

**Step 4**

If you already had a Github remote setup for you applcaition on your local computer and did not setup a remote server to Heroku then you can sync your Github repo with your Heroku applciation by following the instructions below:

Go to your applications dashboard on Heroku's website - Here is the [link](https://dashboard.heroku.com/apps) 
- Select your app,
- Click on the sub menu tab that says "deploly"
- In this section you will see the option "Deployment method" 
- Select "Github"
- Type in the name of your Github repo and save it to connect
- In the option that says manual depoly click "Deploy Branch" - This will lanuch the website live
- You can click on the Open App at the top of the page to visit the website you just deployed

*
Now you can continue to make git comits in your terminal and when you want to update the changes in your live app you click the deploy button in the Heroku website.
There is another way to deply your app by typing the commands in the terminal. Whatever method you choose is based on your preference. I did it this way because this is the flow I chose.
*






