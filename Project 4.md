
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
 
 
 Step 2: Install MongoDB 


 


