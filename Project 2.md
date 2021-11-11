## LEMP STACK IMPLEMENTATION

         
      
   Step 1 : Downloaded and installed gitbash
                  
            Launched gitbash 
                  
            Connected to my EC2 instance through Gitbash with the following command:
                  
            cd Downloads
                  
            ssh -i "ope-ec2.pem" ubuntu@ec2-3-136-236-105.us-east-2.compute.amazonaws.com
                  
        
  Step 2 :  Installing the Nginx Web Server
  
   updated the server package with the command : sudo apt update
   ![Capture 1 updated Nginx web server package](https://user-images.githubusercontent.com/92916632/140903636-dce14019-149a-410e-af9b-d584ec888542.PNG)
             
   Installed nginx with the command : sudo apt install nginx
   ![Capture 2  Installed NGINX](https://user-images.githubusercontent.com/92916632/140904469-9f63857d-a66d-4297-8651-b2ac41bd27de.PNG)
             
   When prompted, i entered Y to confirm the installation of  Nginx
   
   Verified that nginx was successfully installed and running. Ran the command: sudo systemctl status nginx
   ![Capture 3 verified that Nginx was installed](https://user-images.githubusercontent.com/92916632/140906452-78f82f6c-150e-4c90-ac44-d411cf38180c.PNG)
   
   Added a new rule to EC2 configuration to open inbound connection through port 80:
   ![Capture 4 open inbound connection through port 80](https://user-images.githubusercontent.com/92916632/140908291-b32d1965-486a-4b93-aac5-a2ea424d8b51.PNG)
   
   Checked if i could access nginx web server locally & checked if it is now correctly installed & accessibe through 
   my firewall.  Ran the command : curl http://localhost:80  
   ![Capture 5a checked if my nginx web server was accebile](https://user-images.githubusercontent.com/92916632/140955182-f2a06612-d173-464e-a475-383511805d78.PNG)
   
   Tested how Nginx server can respond to requests from the Internet. I copied my AWS public piv4 address & 
   pasted it on my web browser.
   ![Capture 5b  checked my nginx server on my web browser](https://user-images.githubusercontent.com/92916632/140954450-d5bb5010-6938-4100-9573-794aef2aa762.PNG)
   
   Step 3 : Installing MySQL
   
   Acquired & installed MySQL using the command: sudo apt install mysql-server 
   ![Capture 6 installed mysql](https://user-images.githubusercontent.com/92916632/140958601-8f8b8096-81d0-4490-bffd-9ccc3c8025d4.PNG)
   
   When prompted, confirmed installation by typing Y, and then ENTER
   
   
  
  Secured MySQL installation by running the command: sudo mysql_secure_installation
  ![Capture 6b  secured mysql installation](https://user-images.githubusercontent.com/92916632/140960789-98e864ec-f273-43bf-8e08-76e8932e3f03.PNG)
  Chose no when asked to validate password component
  
  Set the password for Mqsql root
  
  Answered Yes for the rest of the questions
  
  Tested to see if i was able to log in to the MqSQL console by typing : sudo mysql
  ![Capture 7 tested to see if i can log in to msql](https://user-images.githubusercontent.com/92916632/140962059-5fc3567b-518b-4c71-b6a6-a360a58022df.PNG)
  
  Typed exit to exit mysql
  
  
  Step 4 : Installing PHP 
  
  I installed PHP and other dependencies with the following command: sudo apt install php-fpm php-mysql
  ![Capture 8 installed PHP](https://user-images.githubusercontent.com/92916632/140984235-0693510d-7623-457c-97bd-15d151434fb8.PNG)
  
  When prompted, i typed Y and pressed ENTER to confirm installation.
  
  Step 5 : Configuring Nginx to Use PHP Processor
  
  Created the root web directory for my domain as follows: sudo mkdir /var/www/projectLEMP
  
  Assigned ownership of the directory with the $USER environment variable, which will reference current system user:
  
  sudo chown -R $USER:$USER /var/www/projectLEMP
  
  Opened a new configuration file in Nginx’s sites-available directory using nano editor: sudo nano /etc/nginx/sites-available/projectLEMP
 
 This created a new blank file. Pasted in the following bare-bones configuration:
 
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
  
Saved and closed the file by typing control x and then y and enter to confirm.

Activated my configuration by linking to the config file from Nginx’s sites-enabled directory with the following command:

sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

Tested my configuration for syntax errors by typing: sudo nginx -t
![Capture 9 Tested my config for syntax error](https://user-images.githubusercontent.com/92916632/140992862-3b6d56d2-dbc7-4a8f-8e2e-c954694f0bf4.PNG)



Disabled default Nginx host that is currently configured to listen on port 80. I ran the following command: 

sudo unlink /etc/nginx/sites-enabled/default

Reloaded Nginx to apply changes. Ran the command : sudo systemctl reload nginx

Created an index html file in that location so that i can test that my new server block works as expected: 

sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

Step 6: Testing PHP with nginx

Tested to validate that nginx can correctly hand .phpfiles off to my processor 

Did this by creating a test PHP file in my document root. Opened a new file called info.php within my document root in my text editor:

sudo nano /var/www/projectLEMP/info.php

pasted the following lines into the new file :

  
     <?php
      phpinfo();
      
   Saved and closed the file by typing ctrl x and then y and then enter
      
   I accessed the page in my web browser by visiting the public IP address i set up in my nginx config file, followed by /info.php
      
   i.e http:// ip address/info.php
      
   I saw a web page containing information about my server
      
   ![Capture 10 PHP test page](https://user-images.githubusercontent.com/92916632/141013343-774d53d2-358a-497e-873a-470cd49b2dcc.PNG)
   
   I removed the file that i created because it contains sensitive info about my php environment & my Ubuntu server:
   
   I ran the command: sudo rm /var/www/projectLEMP/info.php 
   
   Step 7: Retrieving data from MySQL database with PHP
   
   Connected to the Mysql console using the root account : sudo mysql
   
   Created a new database named example_database. Ran the following command: CREATE DATABASE `example_database`;   
   
![Capture 11 created a new database](https://user-images.githubusercontent.com/92916632/141261981-6c24ce12-dda7-4882-acca-951e84b296fd.PNG)

  Created a new user named example_user and granted full priviledges on the database that i have created
  
  For the new user named example_user, i used mysql_native_password as default authentication method. we are 
  
  defining this user's password as password. Ran the command: CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
  
  I gave this user permission over the example_database  database: GRANT ALL ON example_database.* TO 'example_user'@'%';
  
  Exited MySQL shell by typing exit
  
  Tested if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom credentials:
  
  mysql -u example_user -p
  
  requested for password & i typed: password
  
  After logging into the MySQL console, i confirmed that i have access to the example_database by running the command:
  
  SHOW DATABASES;
  
  This gave me the following ouput in a tabular form; output, database, example_database, information_schema
  
  Created a test table named todo_list, ran the following statement from the MySQL console:
  
  CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item_id));
  
  Inserted a few rows of content in the test table: INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
  
  To confirm that the data was successfully saved to my table i ran : SELECT * FROM example_database.todo_list;
  
![Capture 13 inserted row and confimed data was saved](https://user-images.githubusercontent.com/92916632/141281914-bbcb1687-49de-4ad4-9b34-2653c142d3cc.PNG) 

 Exited Mysql console
 
 
 Created a PHP script that will connect to MySQL & query for my content. Created a new php file in my custom root directory by using: 
 
 nano /var/www/projectLEMP/todo_list.php
 
 
 Copied the follwoing content into my  todo_list.php script:
 
   
      <?php
      $user = "example_user";
      $password = "password";
      $database = "example_database";
      $table = "todo_list";
      
      try {
        $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
        echo "<h2>TODO</h2><ol>";
        foreach($db->query("SELECT content FROM $table") as $row) {
          echo "<li>" . $row['content'] . "</li>";
        }
       echo "</ol>";
     } catch (PDOException $e) {
         print "Error!: " . $e->getMessage() . "<br/>";
         die();
     }
     
     
 Saved and closed the file by typing ctrl x, then y, and enter
 

Accessed this page in my web browser by visiting then copying and pasting  the public IP address configured for my website, followed by /todo_list.php: 
 
    http://<IP>/todo_list.php
    
 
 
 ![Capture 14  to do list](https://user-images.githubusercontent.com/92916632/141343838-6d2b0249-8e9a-424f-82dd-068ab28b56c8.PNG)

  
  
  
   
   



  

