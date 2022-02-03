
# Nginx Load Balancing Web Solution with secured HTTPS connection with periodically updated SSL/TLS certificates.

# Task -  Register a new domain name
     
 #     - Configure Nginx as a Load Balancer
     
#       - Secure the website using SSL/TLS encryption

#      - Periodical renewal of SSL/TLS certificate


- Created an EC2 VM based on Ubuntu Server 20.04 LTS and named it Nginx LB 
   
 - Opened TCP port 80 for HTTP connections. Also opened TCP port 443 – this port is used for secured HTTPS connections
 
![Capture 9 opened port 22   port 443](https://user-images.githubusercontent.com/92916632/151833451-ef4c6800-a043-4e48-9610-104cd02c38d7.PNG)

PART 1 REGISTERED A NEW DOMAIN NAME & CONFIGURED NGINX AS A LOAD BALANCER


1. Registered a new domain name at freenom.com
  
  ![Capture 0 register a new domain](https://user-images.githubusercontent.com/92916632/151789392-60aefa59-261c-463f-b3cd-0ea0c2295d3c.PNG)


![Capture 1 tooling ga](https://user-images.githubusercontent.com/92916632/151788150-949887c9-8419-44e3-9092-aa9a7fe31eb2.PNG)

![Capture 2 tooling ga](https://user-images.githubusercontent.com/92916632/151788340-2f1abb2c-5b67-4206-8240-70842fbbf1b2.PNG)

![Capture 3b  review and check out](https://user-images.githubusercontent.com/92916632/151788847-6cb12391-9b31-4c2d-8072-d5f7271a2dc6.PNG)

Screenshot below showing the newly created domain

![Capture 4 showing newly created domains](https://user-images.githubusercontent.com/92916632/151788486-2f5773ac-014b-4870-b55f-f690f3033439.PNG)


2. Created a hosted zone on route 53 on AWS page

![Capture 5 created a hosted zone on route 53](https://user-images.githubusercontent.com/92916632/151797020-fbbba70a-2011-4c8f-814d-8cb6031e413e.PNG)

 
   
  3. Added the newly created domain
   
 ![Capture 6 added the newly created domain name](https://user-images.githubusercontent.com/92916632/151798695-9eda39f1-a249-4ea4-95b2-2a8cf41160cb.PNG) 
 

4.  Connected the route 53 hosted zone to the newly created domain by copying the nameservers from route 53 to the newly created domain. 
 
 From the domain page clicked my domains → manage domain → mangement tools →  nameservers → use custom nameservers → added all name severs → change nameservers
 
 ![Capture 8 change name servers](https://user-images.githubusercontent.com/92916632/151801876-84129e67-fd84-4cab-8a12-d4e10ac1eec1.PNG)

  
5. Created a record in my hosted zone for toolingysf.ga on route 53  to point to Nginx load balancer using load balancer's public IP address

![Capture 10b create record](https://user-images.githubusercontent.com/92916632/151838437-99bd337c-6747-407b-9c3a-fee689c47bf5.PNG)


6. Created another record for www.toolingysf.ga  and also added the load balancer's public IP address

![Capture 11 add record for www](https://user-images.githubusercontent.com/92916632/151839691-319965f0-c5a0-4a7c-87f1-cd5516fa3039.PNG)


The process above ensures that the domain name, load balancer and route 53 have all been connected together

![Capture 12 all connected together](https://user-images.githubusercontent.com/92916632/151841000-6e8cac77-08fa-402f-b3a9-22b0774e6a2f.PNG)



 7. Installed NGINX 

        sudo apt update && sudo apt install nginx -y
        
 
 8. Enabled and started NGINX

        sudo systemctl enable nginx && sudo systemctl start nginx         
 ![Capture 13 enable and start Nginx](https://user-images.githubusercontent.com/92916632/151845362-6c84bd02-0779-4d24-902e-0f21ad0150d6.PNG)
 
 9.  Checked the status of nginx 

          sudo systemctl status nginx

  
  ![Capture 14 status of nginx](https://user-images.githubusercontent.com/92916632/151845960-c1170e07-c8ca-4718-87ba-2f74d320051e.PNG)
  
 19. Created a load balancer config file in Nginx. Added the private IP address of webserver 1, webserver 2, and the newly created domain name

    
            sudo vi /etc/nginx/sites-available/load_balancer.conf
            

![Capture 15 load balancer config file](https://user-images.githubusercontent.com/92916632/151888513-6395bd53-bad6-4743-a60f-502a841964f4.PNG)
![Capture 23b load balancer config file with weight](https://user-images.githubusercontent.com/92916632/151890270-d9934968-1b51-4d20-bd9e-9e9e20e40bf1.PNG)

saved and exit using :wq!


20. Restarted Nginx and made sure the service is up and running

            sudo systemctl restart nginx

            sudo systemctl status nginx
            
![Capture 14 status of nginx](https://user-images.githubusercontent.com/92916632/151890931-a3042391-b56a-40e6-8fa5-75e798af82dc.PNG)
  
   
 20. Removed the default site-enabled file
   
             sudo rm -f /etc/nginx/sites-enabled/default

![Capture 18 remove dafault site-available page](https://user-images.githubusercontent.com/92916632/151866652-4248a47f-393b-4bcc-8591-c27d3a253ed4.PNG)

 21. Checked to see if Nginx was successfully configured

             sudo nginx -t 

![Capture 19 syntax ok](https://user-images.githubusercontent.com/92916632/151870567-70f42f12-bc29-4631-95f8-68bab7464bae.PNG)
 

22. Linked the load balancer config file to the site-enabled file
     
           cd /etc/nginx/sites-enabled/
 
           sudo ln -s ../sites-available/load_balancer.conf .
           
   ![Capture 20 link sites available and config file](https://user-images.githubusercontent.com/92916632/151873263-011b15b6-11f8-4b77-a32c-09f5021ce252.PNG) 
   
               /etc/nginx/sites-enabled$ ls 
            
   ![Capture 21 ls site enabled](https://user-images.githubusercontent.com/92916632/151874278-b35917f5-f6b9-409b-948b-0936451f50ae.PNG)


              /etc/nginx/sites-enabled$ ll           
 
 Screenshot showing the linkage 
            
  ![Capture 22 link betwwn site enabled and config file](https://user-images.githubusercontent.com/92916632/151874453-b73da9b9-0cd1-4f4f-b254-95d1644a53ef.PNG)
  
 23. Reloaded nginx 
  
         sudo systemctl reload nginx         
 
![Capture 22 reload nginx](https://user-images.githubusercontent.com/92916632/151881817-a2bfd908-9fdf-4fd8-818e-364a0747e00e.PNG) 


24. Checked that my Web Servers can be reached from my browser using new domain name using HTTP protocol – http://toolingysf.ga

![Capture tooling webpage](https://user-images.githubusercontent.com/92916632/152041004-09daa304-b0f7-409c-9c39-5b57a99da865.PNG)



PART 2 CONFIGURED SECURED CONNECTION USING SSL/TLS CERTIFICATES  

1.  Installed cerbot

         sudo apt install certbot -y 
         

2.  Installed certbot dependencies 

         sudo apt install certbot python3-certbot-nginx -y
         
3. Checked syntax and reloaded nginx

        sudo nginx -t && sudo nginx -s reload

![Capture 26 test synthax](https://user-images.githubusercontent.com/92916632/152212395-3f5e73bb-bab4-4f21-ae67-0fde49859623.PNG)


4. Requested for an SSL/TLS certificate for the newly created domain to make it secured. 

  
         sudo certbot --nginx -d toolingysf.ga -d www.toolingysf.ga
         
   followed the certbot instructions : 
   
   Entered a valid E.mail address
   
   Agreed to the terms of service
   
   Would you be willing to share your email address with the Electronic Frontier Foundation : Y
   
   
    Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.  : Chose 2 to redirect
    
 ![Capture 27 cerbot certificate](https://user-images.githubusercontent.com/92916632/152241383-9d44b323-f726-4df9-bae6-65df1602dd3e.PNG)
 
 
 5. Accessed my website to confirm secured connection using HTTPS protocol

  ![Capture 28 secured site](https://user-images.githubusercontent.com/92916632/152245159-58d55b90-6a68-4655-b27b-3a7448724bda.PNG)
  
 
![Capture 29 sc showing secured site](https://user-images.githubusercontent.com/92916632/152245248-ea502b42-f198-4487-88c7-95b2e669fc96.PNG)


6.  Set up periodical renewal of  SSL/TLS certificate. Configured a cronjob to run the command twice a day. 

          crontab -e
          
  ![Capture 30 crontab](https://user-images.githubusercontent.com/92916632/152246806-c313e462-8a85-4478-99eb-d86f4bc06577.PNG)
  
  
  Added the following line:
  
        * */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
        
   
  ![Capture  31 crontab config file](https://user-images.githubusercontent.com/92916632/152247143-746169b1-2331-415a-8690-824b68daf59f.PNG) 




  




