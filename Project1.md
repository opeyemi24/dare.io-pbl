
## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

To complete this project, i took the following steps: 

Step 1 : CREATED MY PERSONAL AWS ACCOUNT 
         
        . Signed into my AWS account and launched an EC2 instance of t2 micro family with ubuntu server 20.04 LTS(HVM)
        . Created a new key pair
        . Downloaded and saved my private(pem) key
        . Connected to my EC2 instance through Mobaxterm SSH client
        
        
Step 2 : INSTALLED APACHE AND UPDATED THE FIREWALL
  
         . Installed package update by running command sudo apt update
         . Installed apache by running command sudo apt install apache2
         . Verified that apache2 is running by entering command sudo systemctl status apache2
           Screen shot below showing apache2 status
          
          
          
          
![Capture Apache installation](https://user-images.githubusercontent.com/92916632/139822864-54cfc33b-947d-4521-bd79-0fe74433f0dd.PNG)

        
        .   Added a rule to EC2 configuration to open inbound connection through port 80:
            Screenshot below 
            
            
            
            
            
           
           
 
 ![port80](https://user-images.githubusercontent.com/92916632/139844549-61c7f1b8-c307-45e3-a075-9645a3883ee5.png)
 
 
       .  Verified that my web server is now correctly installed and accessible through my firewall. Opened my EC2 virtual machine
           public IP Address URL on my computer browser. 
         
         Screen shot of the default page below.
          
          
          
          
          
          
        
       
       
       
  ![Capture ubuntu test page](https://user-images.githubusercontent.com/92916632/139849097-9ee438c4-3f7a-4ae0-9f0e-7f9dd9f371f6.PNG)
  
  
  Step 3: INSTALLED MYSQL
          
  . To install mysql,i ran the command sudo apt install mysql-server. When prompted, i confirmed installation by typing Y, and then ENTER.
   
  . I ran the command sudo mysql_secure_installation to secure the installation, and to also remove some insecure default settings.
  
 . I was prompted to configure the VALIDATE PASSWORD PLUGIN. I left validation disabled. Answered Y for yes to continue without enabling.
  
  . I was prompted to select and confirm a password for the MySQL root user.
   
  . Tested connection to MySQL server by running sudo mysql
             
    Screenshot below 
           
  
  
  
  
    
   
   
   
   ![Capture secure myqsl installation](https://user-images.githubusercontent.com/92916632/139917996-bac30559-f7ca-4e69-ba3a-80433c51914f.PNG)
   
   
   
   
   
   
   
   
   ![Capture tested connection to myqsl server](https://user-images.githubusercontent.com/92916632/139918785-38bfdb69-520c-43d6-be9c-053d7b7cbe88.PNG)
            
     . Exit the MySQL console by typing mysql> exit
       
  
    Step 4: Installed PHP
     
  . PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, 
   
  . we need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. we also need libapache2-mod-php 
  
  to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
   
   . To install the 3 above-mentioned package at once,i ran the coomand: sudo apt install php libapache2-mod-php php-mysql 
   
   . To confirm php version I ran the command : php -v
   
   
  
  
  
   
   
   ![Capture PHP version](https://user-images.githubusercontent.com/92916632/139970776-7daaf587-9807-482e-ac46-596ddb35fe3c.PNG)
   
   
   
 
 STEP 5 : CREATED A VIRTUAL HOST FOR MY WEBSITE USING APACHE
 
 . The objective is to setup a domain called ‘projectlamp’

.  Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.

 . I created a directory for project LAMP using the command : sudo mkdir /var/www/projectlamp

.  Assigned ownership of the directory with  current system user using command: sudo chown -R $USER:$USER /var/www/projectlamp

. created and opened a new configuration file in Apache’s sites-available directory : sudo vi /etc/apache2/sites-available/projectlamp.conf

. This created a new blank file. Pasted the following configuration by hitting the i key on my keyboard. Entered the insert mode, and pasted the text:
   
    
    <VirtualHost *:80>
    
    ServerName projectlamp
    
    ServerAlias www.projectlamp 
    
    ServerAdmin webmaster@localhost
   
     DocumentRoot /var/www/projectlamp
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    </VirtualHost>

Saved and closed the file with the following steps:

1. Hit the esc button on the keyboard

2. Type :

3. Type wq (w means write and q means quit)

4. Hit ENTER to save the file


I entered the following command to to show the new file in the sites-available directory: sudo ls /etc/apache2/sites-available

Screen shot showing the new file in the site-available directory



![Screenshot showing new file in site-available directory](https://user-images.githubusercontent.com/92916632/139961294-b5b86adc-92df-4679-ab06-8955ccc7cd2d.PNG)

  Enabled the new virtual host by entering command : sudo a2ensite projectlamp
  
  ![Capture enabled the virtual host](https://user-images.githubusercontent.com/92916632/139961749-2ebc1d6b-4f03-42dd-befc-9b557f6e3ead.PNG)
  



  .I disabled the default website that comes installed with Apache. This is required if you’re not using a custom domain name, 
  
  because in this case Apache’s default configuration would overwrite your virtual host. 
  
  .To disable Apache’s default website i used a2dissite command. I ran the command: sudo a2dissite 000-default
  
  .Screen shot below showing Apache default website disabled
  
  
  
  ![Capture disabled the default website that came with apache](https://user-images.githubusercontent.com/92916632/139964421-9c9cb9db-4a8c-408e-86e0-3d75a6b78fce.PNG)
  
  
  .To ensure my configuration file doesn’t contain syntax errors,i entered the command below:
  
  sudo apache2ctl configtest
  
  
  
![SS to ensure config doesnt have errer  syntax ok](https://user-images.githubusercontent.com/92916632/139964873-4a4acad7-f90f-4b71-9c8e-c484f75c08b3.PNG)


.Reloaded Apache so these changes take effect. I ran the command: sudo systemctl reload apache2


Created an index.html file in that location so that i can test that the virtual host works as expected, by typing this command:

sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-
data/public-ipv4) > /var/www/projectlamp/index.html

i ran the command cd /var/www/projectlamp/ clicked enter

:/var/www/projectlamp$ ls clicked enter and i got index.html
  
  
  
  STEP 6:  ENABLE PHP ON THE WEBSITE 
  
  I need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive
  
  I entered the following command: sudo vim /etc/apache2/mods-enabled/dir.conf
  
  Pasted the following command into the vim editor: 
  
 <IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

I saved and closed the file using vim editor

I Reloaded apache2 so change can take effect. Ran the command : sudo systemctl reload apache2

Created a PHP test script file known as ‘index.php’ to confirm that Apache is able to handle and process requests for PHP files.

Ran the command : vim /var/www/projectlamp/index.php

A blank file was opened.  I added the text below inside the file:

<?php
phpinfo();

I saved and closed the file

Refreshed my EC2 public ip address url page and a PHP default page came up






![Capture php version today](https://user-images.githubusercontent.com/92916632/139969581-e2459a51-9cdf-4498-8cf0-d7574db06d5c.PNG)
            
            
            
            
            
            
            
           

           
          
