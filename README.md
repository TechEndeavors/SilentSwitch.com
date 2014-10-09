CDNSwitch.com
=============

Service that does health monitoring on infrastructure and makes changes to CDN provider configurations. 


Initial focus is on Cloudflare and the monitoring will be simple ping and HTTP header checks. 

#Resources
1. https://www.cloudflare.com/docs/client-api.html

#Development:
1. Determine proper database schema to store the information
2. Build user account creation/login system
3. Prompt user for Cloudflare API key
4. Connect to Cloudflare API and get a list of domains
5. Ask user which domain(s) they want to work with ( https://www.cloudflare.com/docs/client-api.html#s3.2 )
6. Cycle through the domains to get the DNS config and store it in the DB ( https://www.cloudflare.com/docs/client-api.html#s3.3 )
7. Also get the cloudflare config for that zone ( https://www.cloudflare.com/docs/client-api.html#s3.7 )
8. Determine the servers that power the users website. Most of the time, it'll be looking for CNAME or A records for www.example.com or example.com. Narrow the list down to just the IP addresses
9. Ask the users how they want the server(s) monitored (ping/HTTP headers/content on webpage) and let them choose what settings (ping frequency, what header to expect, what content to GREP for, etc)
10. If there is more than one web server, ask the user what should happen if one server goes offline (only choice at the moment is just removing that server from DNS records https://www.cloudflare.com/docs/client-api.html#s5.3 and adding it back when the server comes back up)
11. Ask what to do if there are no servers that are responding anymore (choices are: Let cloudflare display an error, point traffic to [user entry] ip address, point traffic to CDNSwitch hosted status page)
12. Ask user for their notification preferences (checkboxes to determine what kinds of notifications: server down, server unreliable, server up, cloudflare change made) (checkboxes and fields to set communication types: email, sms, webhook)
