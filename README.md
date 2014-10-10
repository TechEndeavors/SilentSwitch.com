CDNBalance.com
=============

CDNBalance is a service that does health monitoring on your infrastructure and makes automated changes to various services to ensure that any problems don't affect your end users experience. 

The initial focus is monitoring your front-end web server(s) and making changes to Cloudflare so server downtime doesn't result in errors being shown to the users. 

Simply sign up for a free account, enter your Cloudflare API key, and follow the wizard. It'll guide you through picking which zones you want to work with, helping us understand your infrastructure, and choosing what you want to do whenever our system detects server downtime. 

Additional features that are planned are load monitoring, ability to add additional servers to your front-end server farm, interoperability with other CDN providers, monitoring Cloudflare itself and changing name server configuration during problems, and automated restarts of servers that are failing health checks. 

#Resources
1. https://www.cloudflare.com/docs/client-api.html

#Development needed until first milestone:
1. Determine proper initial database schema
2. Build user account creation/login system
3. Prompt user for Cloudflare API key
4. Connect to Cloudflare API and get a list of zones under that API key
5. Ask user which zones they want to work with ( https://www.cloudflare.com/docs/client-api.html#s3.2 )
6. Cycle through the zones and get the cloudflare config for that zone ( https://www.cloudflare.com/docs/client-api.html#s3.7 ) - We'll eventually be offering the user some suggestions on how to improve their experience
7. Get the DNS config for that zone and store it in the DB ( https://www.cloudflare.com/docs/client-api.html#s3.3 )
8. Determine the servers that power the users website. Most of the time, it'll be looking for CNAME or A records for www.example.com or example.com. Narrow the list down to just the IP addresses
9. Ask the users how they want the server(s) monitored (ping/HTTP headers/content on webpage) and let them choose what settings (ping frequency, what header to expect, what content to GREP for, etc)
10. If there is more than one web server, ask the user what should happen if one server goes offline (only choice at the moment is just removing that server from DNS records https://www.cloudflare.com/docs/client-api.html#s5.3 and adding it back when the server comes back up)
11. Ask what to do if there are no servers that are responding anymore (choices are: Let cloudflare display an error like it normally does, point traffic to CDNBalance hosted status page, or point traffic to user supplied ip address )
12. Ask user for their notification preferences (checkboxes to determine what kinds of notifications: server down, server unreliable, server up, cloudflare change made) (checkboxes and fields to set communication types: email, sms, webhook)
