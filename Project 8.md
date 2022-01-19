
# LOAD BALANCER SOLUTION WITH APACHE

# Task -  Deploy and configure an Apache load balancer for tooling website solution

# To ensure that users can be served by web servers through the load balancer.
      
 STEP 1  Created an Ubuntu Server 20.04 EC2 instance and named it apache-load balancer 
 
![Capture apache lb](https://user-images.githubusercontent.com/92916632/150131246-96a95657-7f7a-40c1-bc2d-39dc886d57eb.PNG)
 
 STEP 2  Opened TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.
 
![Capture TCP 80](https://user-images.githubusercontent.com/92916632/150133289-7e80ac96-41a8-496b-b445-96e7f6787636.PNG)

 STEP  3 Installed Apache load balancer on apache-lb server and configure it to point traffic coming to LB to both Web Servers:

   Install apache2
   
         sudo apt update
       
         sudo apt install apache2 -y

         sudo apt-get install libxml2-dev
         
 ![Capture sudo apt update](https://user-images.githubusercontent.com/92916632/150133882-da1ae443-2767-41d8-9ba4-f64f0e0b5bf8.PNG)
 
 ![Capture install apache](https://user-images.githubusercontent.com/92916632/150134247-f5abf612-6bc4-4ef6-9303-ff50bbf9a27d.PNG)
 
 ![Capture libmadom](https://user-images.githubusercontent.com/92916632/150136911-75ee5500-18b0-472a-a44f-6b41ac00318e.PNG)
         
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
         
 ![Capture apache status](https://user-images.githubusercontent.com/92916632/150138048-d1532fb7-5f8d-4cb8-b8bb-963345d17199.PNG)
         
  Configured load balancing
  
  Opened the default config file of Apache server with the command below
  
         sudo vi /etc/apache2/sites-available/000-default.conf
         
  Added private IP address of webserver 1 & webserver 2 to  the configuration below
  
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

saved and exited :wq!
 
 Restarted apache server
 
            sudo systemctl restart apache2
            
   4. Verified that my configuration worked – Accessed my load balancer’s public IP address :

            http://<Load-Balancer-Public-IP-Address
            
  ![Capture tooling website with ip address](https://user-images.githubusercontent.com/92916632/150144731-294b6d73-fa95-4f85-961b-6d9fb373e97c.PNG) 
  
   In the previous project (Project-7) i mounted /var/log/httpd/ from  Web Servers to the NFS server. To ensure that each webserver 
   
   has its own directory, i unmounted them. I ran the command below :
  
  
             sudo umount -f /var/log/httpd
             
     
            
  
  5. Opened two ssh consoles for  web Servers 1 and webserver 2 and ran following command:

            sudo tail -f /var/log/httpd/access_log
            
   Screenshot showing http get requests from webserver 1 and 2 
            
  ![Capture get request 1](https://user-images.githubusercontent.com/92916632/150034900-3c8befbc-7c6a-4ec2-95aa-924605ff9963.PNG)
  
  
  ![Capture get request 2](https://user-images.githubusercontent.com/92916632/150035116-71108312-592e-4b4a-80bf-25d80644d8bc.PNG)
  
  
 Refreshed my browser page http://<Load-Balancer-Public-IP-Address several times to ensure that both servers receive HTTP GET 
    
 requests from my load balancers.
 
 
 The number of requests to each server will be approximately the same since we set loadfactor to the same value for 
                                                                   
 both servers. It means that traffic will be distributed  evenly between them.
          
          
          
          
