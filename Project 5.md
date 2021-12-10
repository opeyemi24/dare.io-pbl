
# TASK –  To implement a Client Server Architecture using MySQL Database Management System (DBMS)

STEP 1 : Created and configured two Linux-based virtual servers (EC2 instances in AWS).

Server A name - 'mysql server'

Server B name - `mysql client'

Screenshot showing the 2 instances created

![Capture instances](https://user-images.githubusercontent.com/92916632/145488233-956b3c76-1365-4ab4-9358-5c0e2979560d.PNG)

STEP 2 : On mysql server linux server, i installed myqsl server software with the command : sudo apt update -y
 
                                                                               sudo apt install myqsl-server -y
 Screenshot showing the installation of myqsl server software
 
 ![capture 1 ubuntu update server](https://user-images.githubusercontent.com/92916632/145607565-587ea132-4354-468f-b8e7-94dc039dbc5f.PNG)
 
 ![Capture 2 install mqsql server](https://user-images.githubusercontent.com/92916632/145607804-8b480a9c-b06e-4ff7-90e9-87504695df26.PNG)

                                                                                
                                                                                  
 
                                                                  
  Enabled the service with the command : sudo systemctl enable mysql
  
  ![Capture 3 enabled the service](https://user-images.githubusercontent.com/92916632/145608560-57c04012-a2b6-4153-b87e-afe51bc58747.PNG)

  
 STEP 3 : On mysql client linux server, i installed mysql client software with the command :  sudo apt update -y
  
                                                                              sudo apt install mysql-client -y
 Screenshot showing the installation of mysql client software
 
 ![Capture 4 update ubuntu client](https://user-images.githubusercontent.com/92916632/145609668-e5df6641-19bc-497d-b361-f3994f3f16d9.PNG)
 
 ![Capture 5 install mysql client](https://user-images.githubusercontent.com/92916632/145609853-d4bb9d3c-8747-431c-b2c4-a6ab15d4cdfe.PNG)

 
 STEP 4 : Created a new entry(myql) in the inbound rule of mysql server security group.  mysql server uses TCP port 3306 by default
 
 I also added the private IP address of mysql client  into  mysql server's inbound rule
 
![Capture 6 edit inbound rule](https://user-images.githubusercontent.com/92916632/145629904-932ca6f6-cc45-4469-9341-d8471ef2b171.PNG) 

To obtain the client's private IP address, we can either copy directly from the AWS console or 

run the command : ip addr show  

![Capture 7 show ip address](https://user-images.githubusercontent.com/92916632/145630927-0809d825-34fe-4032-bb2e-31e07c75a9ab.PNG)


 
 
 
  
  
  
  
  
