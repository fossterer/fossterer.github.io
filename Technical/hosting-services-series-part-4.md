# Hosting Services Series - Multiple subdomains without complicating DNS configuration 

* Let there be an A record that points root domain to server IP address 
* Then have a CNAME of wildcard subdomain to root domain. Now in nginx config, define as many server blocks as needed to different app deployment folders