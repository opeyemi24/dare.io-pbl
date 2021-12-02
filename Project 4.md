
## MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

## In this project, i implemented a simple Book Register web form using MEAN stack.

MEAN Stack is a combination of following components:

1. MongoDB (Document database) – Stores and allows to retrieve data
2. Express (Back-end application framework) – Makes requests to Database for Reads and Writes
3. Angular (Front-end application framework) – Handles Client and Server Requests
4. Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user

Connected to the server through mobaxterm SSH client by taking the following steps:

Created a new session by clicking session

Clicked the SSH tab & pasted my public IP address into the remote host box

Specified username as Ubuntu

Clicked on advanced SSH settings,clicked on use private key,and uploaded my PEM key and then clicked ok


STEP 1 : INSTALLED NODEJS

Updated Ubuntu with the command  :  sudo apt update

![Capture 1 updated ubuntu](https://user-images.githubusercontent.com/92916632/144457814-8f8ba78e-bfa6-4212-bde5-28b29e634fec.PNG)

Upgraded Ubuntu with the command :  sudo apt upgrade

![Capture 2 upgraded ubuntu](https://user-images.githubusercontent.com/92916632/144458284-189237c2-b1b4-40be-9db0-95078bcce57f.PNG)


Added certificates : 

sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

 Installed Nodejs with the command : sudo apt install -y nodejs
 
 Confirmed the version of Nodejs installed by running : node  --version
 
 ![Capture 3 installed Nodejs](https://user-images.githubusercontent.com/92916632/144459108-d3adfdfe-e7df-49ee-b406-090727a9d411.PNG)
 
 
 Step 2: INSTALLED MONGODB
 
 Added MongoDB key to the key server by pasting the following command : 
 
 sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
 
 
 Added a repository for MongoDB with the following command : 
  
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list


![Capture 4 Added Mongodb to kerserver   added a repository for Mogodb](https://user-images.githubusercontent.com/92916632/144470512-29f1a322-fcd5-46db-a4f9-c421e3d224b5.PNG)


Installed MongoDB with the following command :

sudo apt install -y mongodb

![Capture 5 installed Mongodb](https://user-images.githubusercontent.com/92916632/144470954-13a41933-cac1-4589-aff8-038165f09686.PNG)

Started the server : sudo service mongodb start

Verified that server is up and running : sudo systemctl status mongodb

![Capture 6 started server and verified that service is up and running](https://user-images.githubusercontent.com/92916632/144471269-2f0c5888-7b91-4eac-a145-ef2ec746e910.PNG)


Installed npm-Node package manager with the command below :

                                     sudo apt install aptitude
                                     
                                     sudo aptitude install -y npm
                                     
![Capture 7 installed npm](https://user-images.githubusercontent.com/92916632/144471833-8e4dbe68-a2f9-42ea-b887-5fdb5af4a02f.PNG)


 
 


 


