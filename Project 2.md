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

