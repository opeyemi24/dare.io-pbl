
# TASK –  To implement a Client Server Architecture using MySQL Database Management System (DBMS)

STEP 1 : Created and configured two Linux-based virtual servers (EC2 instances in AWS).

Server A name - 'mysql server'

Server B name - `mysql client'

Screenshot showing the 2 instances created

![Capture instances](https://user-images.githubusercontent.com/92916632/145488233-956b3c76-1365-4ab4-9358-5c0e2979560d.PNG)

STEP 2 : On mysql server, i installed myqsl server software with the command : 

           sudo apt update -y
 
           sudo apt install myqsl-server -y
 
 Screenshot showing the installation of myqsl server software
 
 ![capture 1 ubuntu update server](https://user-images.githubusercontent.com/92916632/145607565-587ea132-4354-468f-b8e7-94dc039dbc5f.PNG)
 
 ![Capture 2 install mqsql server](https://user-images.githubusercontent.com/92916632/145607804-8b480a9c-b06e-4ff7-90e9-87504695df26.PNG)

                                                                                
                                                                                  
                                                                  
  Enabled the service with the command : 
  
               sudo systemctl enable mysql
  
  ![Capture 3 enabled the service](https://user-images.githubusercontent.com/92916632/145608560-57c04012-a2b6-4153-b87e-afe51bc58747.PNG)

  
 STEP 3 : On mysql client server, i installed mysql client software with the command :  
 
                sudo apt update -y
  
                sudo apt install mysql-client -y
 
 ![Capture 4 update ubuntu client](https://user-images.githubusercontent.com/92916632/145609668-e5df6641-19bc-497d-b361-f3994f3f16d9.PNG)
 
 ![Capture 5 install mysql client](https://user-images.githubusercontent.com/92916632/145609853-d4bb9d3c-8747-431c-b2c4-a6ab15d4cdfe.PNG)

 
 STEP 4 : Opened mysql port on mysql server.  I also added the private IP address of mysql client into mysql server's inbound rule
 
![Capture 6 edit inbound rule](https://user-images.githubusercontent.com/92916632/145629904-932ca6f6-cc45-4469-9341-d8471ef2b171.PNG) 


To obtain the client's private IP address, we can either copy directly from the AWS console or run the command:

           ip addr show  

![Capture 7 show ip address](https://user-images.githubusercontent.com/92916632/145630927-0809d825-34fe-4032-bb2e-31e07c75a9ab.PNG)


STEP 5 : Created a user and database for mysql server

Firstly, i had to secure the installation of mysql server by running the security script : 

            sudo mysql_secure_installation

I entered n for No when asked to set up validate password component. In production environment, it is adviced that we set 

up validate password component for security purposes.


Set the password for root : password 

Screenshot showing mysql secure installation

![Capture 8 secure installation](https://user-images.githubusercontent.com/92916632/145674407-4a1eee03-a74a-452b-938f-133b74c7ab25.PNG)


Remove anonymous users : Y

Disallow root login remotely : Y 

Remove test database and access to it : Y

Reload priviledge tables now : Y

![Capture 9](https://user-images.githubusercontent.com/92916632/145674523-4bbb9008-090b-47a8-931b-cc714374654f.PNG)

 Ran the follwing command :      
 
                sudo mysql 

Created a user named remote_user : 

               CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password' ;

Created the database :      

               CREATE DATABASE test_db ;

Granted privileges : 

                GRANT ALL ON test_db. * TO 'remote_user'@'%' WITH GRANT OPTION;

Flush privileges:
                
                 FLUSH PRIVILEGES;
 
 
 ![Capture 10d](https://user-images.githubusercontent.com/92916632/145674660-f3738816-684c-4e03-80ef-d08e9212246b.PNG) 
 ![Capture 10 c  flush privileges](https://user-images.githubusercontent.com/92916632/145674616-2f1f83c6-8adc-469b-97d0-9b22042d716d.PNG)
 
 
 STEP 6: Configured mysql server to allow connections from remote host
 
 
                 sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
 
 I replaced '127.0.0.1' with '0.0.0.0' on the bind address. I did that by pressing esc, deleting the 127.0.0.1 on bind address, 
 
 then pressed insert and then typed 0.0.0.0, then typed :wq! enter to save
 
  Screenshot showing the initial bind address
 
 ![Capture 11 ip 127 0 0 0](https://user-images.githubusercontent.com/92916632/145778652-3d3d7554-4518-4ec8-972f-4ccdf99aa81c.PNG)
 
  Screenshot showing the edited bind address
 
 ![Capture 12 bind address](https://user-images.githubusercontent.com/92916632/145779342-e70ee021-862a-4982-8ec9-ffce015122fd.PNG)
 
 From mysql client, i connected remotely to mysql server database by running the command ;
 
             sudo mysql  -u remote_user -h private ip address of mysql server  -p 
 
 -u signifies user i.e username for mysql server, -h signifies host, -p signifies password prompt
 
 requested for password, and i entered the password : password
 
 Screenshot showing remote connection from mysql client to mysql server database
 
 ![Capture 13 log in to mysql from client](https://user-images.githubusercontent.com/92916632/145785264-334fb6bf-8336-44b3-aa58-5e1c0caf54ca.PNG)
 
 Checked that i have successfully connected to the remote mysql server and can perform SQL queries from mysql client.
 
 Ran the following command from mysql client : 
 
          Show databases; 
 
 ![Capture 14](https://user-images.githubusercontent.com/92916632/145786965-c01bde21-42c6-4d95-b61f-fa3e789877cd.PNG)

 
 
 
 
 
 
 
 
 
 

 
 
 
  
  
  
  
  
