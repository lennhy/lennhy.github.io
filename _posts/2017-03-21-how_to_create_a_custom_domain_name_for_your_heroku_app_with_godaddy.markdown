---
layout: post
title:  "How to create a custom domain name for your Heroku app with GoDaddy"
date:   2017-03-21 21:18:57 +0000
---


This blog assumes that you alreayd have a Heroku acccount setup:
This is a continuation of http://codelenn.com/2017/03/20/how_to_deploy_a_sinatra_rack_based_application_to_heroku_github_sync/
Note: By default, any app on Heroku is accessible via its Heroku Domain, which has the form [name of app].herokuapp.com

**Heroku**

1 Login to your Heroku account in your terminal and enter your credentials:

```
heroku login 
```


**Using a custom Domain**

2 To add a custom domain use the domains:add command in the Heroku CLI (In the terminal using Heroku commands instead of linux) :

```
heroku domains:add www.domain.com
Adding www.domain.com to ⬢ superman-z-9790... done
 ▸    Configure your app's DNS provider to point to the DNS Target www.domain.com.herokudns.com.
 ▸    For help, see https://devcenter.heroku.com/articles/custom-domains

The domain www.domain.com has been enqueued for addition
 ▸    Run heroku domains:wait 'www.domain.com' to wait for completion
[18:40:14] (master) Kener-webapp
```

**GoDaddy**

1 Go to GoDaddy website and login into your GoDaddy account.
2 Click on Domains button
3 Click on the link "manage dns" next to the domain

4 For the row with a column Type: "CNAME" and Name: "www" paste in the heroku url (name of app].herokuapp.com)

5 If you don't see a row with these attributes create one by clicking the add button and put in the CNAME for the Type column and "www" the Name column.

6 Scroll down to the forwarding section:
7 Incase someone types your domain name with the www. at the start you can make it forward to the "www" address.
8 To do this click "add" next to domain under the forwarding section.
9 It will have the option "Forward to" - for this type the "www.customdomainname.com"
10 It will also have the option "Forward type" - for this Permanent (301) radio button option
11 For "Settings" click Forward only
12 Click save 

It will take a couple minutes for your domain to update. Once it is updated you visit your website via your custom domain url. 


