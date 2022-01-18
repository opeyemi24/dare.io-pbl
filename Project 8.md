
# LOAD BALANCER SOLUTION WITH APACHE

# Task -  Deploy and configure an Apache load balancer for tooling website solution

# To ensure that users can be served by web servers through the load balancer.
      
 STEP 1  Created an Ubuntu Server 20.04 EC2 instance and named it apache-lb 
 
 STEP 2  Opened TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.

 STEP  3 Installed Apache load balancer on apache-lb server and configure it to point traffic coming to LB to both Web Servers:

   Install apache2
   
         sudo apt update
       
         sudo apt install apache2 -y

         sudo apt-get install libxml2-dev
         
   Enabled the following modules
   
          sudo a2enmod rewrite
         
          sudo a2enmod proxy
         
          sudo a2enmod proxy_balancer
        
          sudo a2enmod proxy_http
         
          sudo a2enmod headers
        
          sudo a2enmod lbmethod_bytraffic
          
  Restarted apache2 service
  
          sudo systemctl restart apache2
          
  Made sure apache2 is up and running
  
         sudo systemctl status apache2
         
  Configured load balancing
  
         sudo vi /etc/apache2/sites-available/000-default.conf
         
  Added the configuration below
  
         <Proxy "balancer://mycluster">
               BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
        
![Capture load factor](https://user-images.githubusercontent.com/92916632/150034170-8a4b849c-c2a8-4a87-bea0-f1f2d5a9b530.PNG)
 
 Restarted apache server
 
            sudo systemctl restart apache2
            
   4. Verified that my configuration worked – Accessed my load balancer’s public IP address :

            http://<Load-Balancer-Public-IP-Address
            
   5. Opened two ssh consoles for  web Servers 1 and webserver 2 and ran following command:

            sudo tail -f /var/log/httpd/access_log
            
  ![Capture get request 1](https://user-images.githubusercontent.com/92916632/150034900-3c8befbc-7c6a-4ec2-95aa-924605ff9963.PNG)
  
  
  ![Capture get request 2](https://user-images.githubusercontent.com/92916632/150035116-71108312-592e-4b4a-80bf-25d80644d8bc.PNG)
 
          
          
          
          
