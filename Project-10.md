# Project 10: LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

CONFIGURE NGINX AS A LOAD BALANCER

Uninstall Apache from the existing Load Balancer server, or create a fresh installation of Linux for Nginx.

Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)

Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses

REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

Make necessary configurations to make connections to our Tooling Web Solution secured!

In order to get a valid SSL certificate – you need to register a new domain name, you can do it using any Domain name registrar – a company that manages reservation of domain names.

Registered a domain at Domain.com

![Order review](image/Order_review.PNG)

![Order complete](image/Order_complete.PNG)

![Registered a domain name](image/Domain_Name_registered.PNG)

Link Route 53 to your domain name

![Route53 dashboard](image/Route53_dashboard.PNG)

![Create hosted zone Route53](image/Create_hosted_zone_Route53.PNG)

Copy all available nameservers on Route 53 to the custom nameservers entry form in the domain create on Domain.com, and click on change nameservers.

![New nameservers added](image/New_nameservers_added.PNG)

Create record for domain name

![Create record for domain name](image/Create_record_for_domainName.PNG)

Complete the task by pointing the record to the Load Balancer public IP

![Create record for domain name successful](image/Create_record_for_domainName_Successful.PNG)

Also create a record for the www

![create a record for www](image/Create_record_for_www.PNG)

Complete the task by pointing the record to the Load Balancer public IP

![Alt text](image/Create_record_for_www_Successful.PNG)

Spin-up a new Ubuntu server for the Load Balancer, so the Ec2 dashboard looks like this:

![Project-10 running instances ](image/Project10_AWS_instances.PNG)

SSH connect to the server

Edit inbound rules for the LB server

![Edit inbound rules for LB server](image/Edit_inbound_rules_NginxLB.PNG)

Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

Update the instance and Install Nginx

`sudo apt update && sudo apt-get install nginx`

![Update LBserver and install Nginx](image/Update_LBserver_and_install_Nginx.PNG)

Configure Nginx LB using Web Servers’ names defined in /etc/hosts

Open the default nginx configuration file

`sudo vi /etc/nginx/nginx.conf`

![Open up default nginx configuration file](image/Open_default_nginx_config_file.PNG)

![Edit default nginx configuration file](image/Edit_default_nginx_config_file.PNG)

Restart Nginx and make sure the service is up and running

`sudo systemctl restart nginx`

`sudo systemctl status nginx`

![Nginx is running](image/Nginx_started_sucessfully.PNG)

To remove the default site so that the reverse proxy will be redirecting to our new configuration file

`sudo rm -f /etc/nginx/sites-enabled/default`

Check if Nginx is successfully configured

`sudo nginx -t`

![Remove the default site](image/remove_default_enabled-sites.PNG)

CD into the Nginx/site-enabled directory/
then link the site available to the sites enabled

`cd /etc/nginx/sites-enabled/`

`ls`

`sudo ln -s ../sites-available/load_balancer.conf .`

`ls`

`ll`

![Linking the sites](image/Linkng_sites-aailable_to_LB_config_file.PNG)

`sudo systemctl reload nginx`

Accessing the tooling website via the browser using the registered domain name

![Accessing the tooling website](image/Accessing_webservers_via_the_tooling_website.PNG)

Notice it's not secured. To make it secure, install certbot

`sudo apt install certbot -y`

![install certbot](image/Install_certbot.PNG)

Install certbot dependency

![Install certbot dependencies](image/Install_certbot_dependencies.PNG)

`sudo nginx -t && sudo nginx -s reload`

![Check syntax and reload Nginx](image/Check_syntax_and_reload.PNG)

Create a certificate to secure the tooling site

`sudo certbot --nginx -d toolingtun.me -d www.toolingtun.me`

![Generating certificates](image/Certificate_successfully_generated.PNG)

![Site secured](image/Secured_toolingtun-me-website.PNG)

![Site has a valid certificate](image/Secured_toolingtun-me-website2.png)

Create a crontab to automatically renew the certificate on expiration

 Test renewal command in dry-run mode

`sudo certbot renew --dry-run`

Best practice is to have a scheduled job that will run renew command periodically. Configure a cronjob to run the command twice a day.

To do so, edit the crontab file with the following command:

`crontab -e`

![Open crontab file](image/creating_crontab_for_Ubuntu.PNG)

Add following line:

`* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1`

![edit crontab](image/crontab_edit.PNG)
