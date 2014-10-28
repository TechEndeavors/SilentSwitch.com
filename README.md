SilentSwitch.com
=============

SilentSwitch is a service that does health monitoring on your infrastructure and makes automated changes to various services to ensure that any problems don't affect your end users experience. 

The initial focus is monitoring your front-end web server(s) and making changes to Cloudflare so server downtime doesn't result in errors being shown to the users. 

Simply sign up for a free account, enter your Cloudflare API key, and follow the wizard. It'll guide you through picking which zones you want to work with, helping us understand your infrastructure, and choosing what you want to do whenever our system detects server downtime. 

Additional features that are planned are load monitoring, ability to add additional servers to your front-end server farm, interoperability with other CDN providers, monitoring Cloudflare itself and changing name server configuration during problems, and automated restarts of servers that are failing health checks. 

#Resources
1. https://www.cloudflare.com/docs/next/

#Development needed until first milestone:
User experience description for Minimum Viable Product:
The user hits the landing page which explains the service and shows the pricing. On the menu bar, there is a button to log in and a button to create an account. 

When they create an account for the first time, it'll just ask for an email address and a password. Then it should bring up some kind of wizard to help them get setup. 

##Wizard: Selection of a domain to monitor
The wizard asks them to enter their domain name that they want monitored. If the user has already entered their cloudflare API key and the wizard is just being run again, we should also present a list of domains in their Cloudflare account that haven't yet been added below the text box. Upon entering the domain name in the text box, a DNS check is done to determine the NS records for the domain to verify that it is hosted by Cloudflare. It would be nice to have some nice DHTML action so after the domain name is entered, there could be some text that appears below the input box that says "Domain is hosted on Cloudflare" before the next button appears. If they just click a domain name that we know is in their Cloudflare account, it should go to the next screen 

I suppose it would be a good idea to also have a "skip wizard button" that takes them to their dashboard (more on that later). 

##Wizard: Entry of Cloudflare API key
Upon clicking next, the wizard will see if the user has already entered their Cloudflare API key. If not, the user is asked for their Cloudflare API key with a little set of instructions on where to get it. Once it's entered and they click a button, it'll verify the API key and verify that the domain they entered is in their Cloudflare account. If the API key works and the domain is found, it'll also submit a request for all the DNS records for that domain. If their API key is already entered it skips that screen.

If there is some kind of error, it'll bring them back to the API key entry page with an error message explaining what went wrong. 

##Wizard: Selection of servers to monitor
The next screen which will show what servers we've determined they're for that domain. This is basically all the A, AAAA, and CNAME records. It'll ask them to place a check next to the listed server(s) that are their web servers. 

##Wizard: Selection of monitoring methods
The next screen will ask them what kind of monitoring they want to do. The options will be "Host monitoring" (ping), "Web server monitoring" (web server header checks), and "Advanced Monitoring" (page content check). They should be able to choose multiple categories.

##Wizard: Selection of actions to perform on failure
The next screen will ask them what they want to do when any of the servers fails. If there is just one server, the text will read "If your server fails the health check, what should we do?". The check boxes will be "Change IP address to: ____", "Forward users to URL: ____", and "Forward users to custom error page". 

If there is more than one server, there should be two sections. The first will ask "If one server fails, what should we do?" and the only option at the moment will be "Temporarily remove the failing server." The second section will say "If all servers fail the health checks, what should we do?" The check boxes will be "Change IP address to: ____", "Forward users to URL: ____", and "Forward users to custom error page". 

##Wizard: Selection of Notification Options
The next page will ask the user if they want any kind of notification when these actions take place. They'll be able to choose multiple options including "Email: ___", "SMS Message: ___", and "Phone Call: ___". The form should check to see if they've already entered any of those values previously in their account profiles but also let them enter a free-form value. If they select or enter an email address that is the same domain that they're monitoring, it might be worth telling the user to make sure that if the server fails that they can still receive email. Eventually we can make that check a bit smarter by checking the IP address of the mail server to make sure it's different than the IP address of the servers being monitored.

##Wizard: Confirmation Screen
The next screen will be a confirmation of all the settings they just picked. If their cloudflare account listed other domains, it should let them restart the wizard to add another monitored domain. If we only detected one domain, it should take them to their user dashboard. 

##User Dashboard
At the top of the user dashboard should be a button to launch the wizard again. We'll probably also add some kind of "Quick Entry" drop-down form eventually that lets them just select an unmonitored domain and choose the monitoring, action, and notification options they've already configured. 

Below that should be a message notification box that just tells them things like "Enter your phone number on your user profile page to be able to receive SMS notifications" or "There are 8 days left in your trial". 

Below that is the list of domains being monitored and managed. It will simply list the domain name and some very basic details. Clicking the domain name should bring the user to a page to view all of the settings and to edit them. 

I'm not sure what should be included in the quick listing. Just including the basics might be information overload. Ideally, it would be nice to be able to glance to see the domain name, the provider (cloudflare), how many servers are being monitored, what kind of actions occur during server failure, and if notifications are turned on or not. 

Maybe the user dashboard can have a "details" button that expands all of the listings to include a lot more information but the default simple display just shows a table like:

Domain Name  | Status | Monitoring | Contingency | Notify | Activity
-----------|---------|-----------|------------|----------|------------
example1.com |      ✓ |      5 min |   Available |      ✓ | 1/1 Servers Healthy  
example2.com |      X |     15 min | In Progress |      ✓ | 1/1 Servers Offline
example3.com |      ✓ |     10 min | Unavailable |      X | 2/2 Servers Healthy  
example4.com |      X |     15 min | In Progress |      ✓ | 1/3 Servers Offline
example5.com |      X |     15 min | In Progress |      ✓ | 2/3 Servers Offline

Or, even simpler:

Domain Name  | Health | Next Check 
-----------|---------|-----------
example1.com |   100% |      5 min    
example2.com |     0% |      1 min 
example3.com |   100% |      5 min  
example4.com |    66% |      1 min 
example5.com |    33% |      1 min 

 
##Database Notes
I'm studying the data available in the Cloudflare API to get an idea of the best database schema that'll be needed to store the information. 

