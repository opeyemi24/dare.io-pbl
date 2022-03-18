
# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. 

# Continous Integration pipeline for tooling website 

Step 1 – Install Jenkins server

1. Created an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"

2. Installed JDK (since Jenkins is a Java-based application)

       sudo apt update
       
       sudo apt install default-jdk-headless
       
3. Installed Jenkins

       wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
       
       sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
             
       sudo apt update
       
       sudo apt-get install jenkins

![image](https://user-images.githubusercontent.com/92916632/150677076-108d226d-3961-441c-9b2d-e2c4bff15bcd.png)

       
- Made sure Jenkins is up and running
  
        sudo systemctl status jenkins
        
![Capture 2 status jenkins](https://user-images.githubusercontent.com/92916632/150677178-4c8cb1c3-a391-4f20-aac2-4335f8ff8f1f.PNG)

        
 4. Opened TCP port 8080. By default Jenkins server uses TCP port 8080

![image](https://user-images.githubusercontent.com/92916632/158980694-189e2e0b-578a-492a-a615-8ec37af4ede4.png)





5. Performed initial Jenkins setup


- From my browser, i accessed :

          http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
  

- I was prompted to provide a default admin password
  
 ![Capture jenkins home page](https://user-images.githubusercontent.com/92916632/150677479-0e0167dd-1075-4331-8edb-6d1e4cdba5bc.PNG) 
  

  
 -  Retrieved password from my server:
  
         sudo cat /var/lib/jenkins/secrets/initialAdminPassword

![Capture 4 initial admin password](https://user-images.githubusercontent.com/92916632/150677646-83163612-486f-462c-9acb-3be788a74c92.PNG)

![Capture jenkins home page 2](https://user-images.githubusercontent.com/92916632/150677814-2cc5cf1c-9fa2-4592-9e4f-f548fa75cb0a.PNG)


 
- Was prompted to choose plugings to install – I chose suggested plugins.
 
 ![Capture 3 suggested plug ins](https://user-images.githubusercontent.com/92916632/150677349-00e378d9-4b74-476e-bdd0-845612c45ac6.PNG)

 
 
 
 Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks
 

1. Enabled webhooks in my tooling gitHub repository settings with following process : 

Settings - webhooks - add webhooks - paste jenkins server public ip address - choose application/json - just push the event - add webhook

![Capture 7 add webhooks](https://user-images.githubusercontent.com/92916632/150678029-a4cd0984-fd37-4123-91fd-f87da62285c5.PNG)



![Capture 8 add web hooks](https://user-images.githubusercontent.com/92916632/150677953-59b7c071-e599-4fb5-ad76-20583c8af88a.PNG)





2. From Jenkins web console, clicked "New Item" and created a "Freestyle project", named it project 9 and saved 

![Capture 9 free style project](https://user-images.githubusercontent.com/92916632/150678616-b0b4abd1-b222-42c4-88ff-c4e05dde96fa.PNG)



- Connected my gitHub repository to jenkins. Copied the github repository url 


![Capture 10 repository url](https://user-images.githubusercontent.com/92916632/158991055-6aabce44-919d-4ae8-9936-7ed2c77edcc2.PNG)




 In the configuration of my jenkins freestyle project, chose git repository, pasted the github repository url and saved. 
 
 ![image](https://user-images.githubusercontent.com/92916632/158990583-982e4951-120e-4d65-8092-a6fe113f3d9d.png)

 

- Configure global security

![Capture configure global security](https://user-images.githubusercontent.com/92916632/157860419-6ee9223d-2470-4164-a16a-1d32464de7d6.PNG)

 
 

- Enabled proxy compatibility and saved 

![Capture enable proxy compabitibility](https://user-images.githubusercontent.com/92916632/157860460-c53364a8-8b8e-4fa0-a78f-dcae7bcc8fbe.PNG)


- Added credentials (username and password for github repository)
 
![Capture add credentials and save](https://user-images.githubusercontent.com/92916632/157861647-da53dd4a-b8a6-4d4e-bdeb-9c679c25c933.PNG)


![Capture add credentials](https://user-images.githubusercontent.com/92916632/157861699-aec784b9-b887-4503-87d4-cd98a2f4d21a.PNG)


- select the credentials created and save

![Capture credentials created](https://user-images.githubusercontent.com/92916632/157862628-e654bb52-b913-42f0-b0ee-84c59d488954.PNG)


- Ran a build manaually by clicking build now 


![Capture build noww](https://user-images.githubusercontent.com/92916632/157863914-406e550b-e85c-4f82-805a-0b9ef6d740ff.PNG)


- Verified the console output 

![Capture build- console output](https://user-images.githubusercontent.com/92916632/157863095-fa5abaa6-79c1-4a18-ba2a-71ecb9dc777a.PNG)




Step 3 Click "Configure" your job/project and add these two configurations : 

1. Configure triggering the job from GitHub webhook:

  
  ![Capture build triggers](https://user-images.githubusercontent.com/92916632/157864567-657acc45-78c5-425c-b682-abd7cc22986a.PNG)
  
  
  2. Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".
   
 Clicked post build actions, archive the artifacts, added **  and saved 
 
    
![Capture archive the artifacts](https://user-images.githubusercontent.com/92916632/157866129-4f8fda6f-74f5-4ce8-8eb4-fe4324647891.PNG)


- Make some changes to any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.


![Capture added new line to readme file](https://user-images.githubusercontent.com/92916632/157866907-b44444e5-8c28-449c-93cc-22a703365c60.PNG)

- A new build was launched automatically (by webhook) and we see its results – artifacts, saved on Jenkins server.


![Capture console output  2a](https://user-images.githubusercontent.com/92916632/157868075-3d9fc171-d003-42dc-8ce5-6ec270d05f06.PNG)


- Console output

![Capture console output 2b](https://user-images.githubusercontent.com/92916632/157868250-cba78f83-54aa-4a63-963b-aa7fde5c9ec2.PNG)


Screenshot showing the artifacts generated as a result of the latest build 


![Capture built artifacts](https://user-images.githubusercontent.com/92916632/157869084-274684e0-d19c-49e8-9de4-91a3682ec26f.PNG)




Step 4  Configured jenkins to copy files to NFS server via ssh
 
 Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our /mnt/apps directory in the NFS server. 
 
 
1. Installed "Publish Over SSH" plugin : 

   On main dashboard selected "Manage Jenkins" and choose "Manage Plugins" menu item
   
   On "Available" tab searched for "Publish Over SSH" plugin and installed it
   
 
 ![Capture 30 publish over ssh](https://user-images.githubusercontent.com/92916632/158681272-dac142c6-8728-47a0-bcae-abb8e95b3eb7.PNG)

   

2. Configured the job/project to copy artifacts over to NFS server

    On main dashboard selected "Manage Jenkins" and chose "Configure System" menu item.
    
 
 Scrolled down to Publish over SSH plugin configuration section and configured it to be able to connect to my NFS server:
 
 
1.  Provided a private key (content of the AWS .pem key used to connect to NFS server via SSH )


![Capture 31 AWS private key](https://user-images.githubusercontent.com/92916632/158682881-ef14ad4a-7084-4712-b3aa-ff9c100c3be5.PNG)


2. name  - NFS  

3. Hostname –  private IP address of  NFS server

4. Username – ec2-user (since NFS server is based on ec2 with rhel 8)

5. Remote directory – /mnt/apps since our web servers use it as a mounting point to retrieve files from the NFS server

   Ensured that TCP port 22 on NFS server is open to receive SSH connections.
  

-  Tested the configuration and made sure the connection returns success. saved the configuration
  
   
  ![Capture 32 test configuration](https://user-images.githubusercontent.com/92916632/158684354-da5b4e60-c1b0-4f80-9ef9-da26ffbd2eea.PNG)

  
  
- Opened my jenkins job/project configuration page and clicked add "Post-build Action", clicked send build artifacts over ssh 
 
 
 ![Capture 33 post build actions](https://user-images.githubusercontent.com/92916632/158698309-cf59e913-1fbd-498b-b626-7b08be78f083.PNG)
 
 ![Capture 34 post build actions](https://user-images.githubusercontent.com/92916632/158698929-4d549499-f743-482b-be90-0a6774056be9.PNG)


 
 - Configured it to send all files produced by the build into the previously defined remote directory (/mnt/apps)

In this case we want to copy all files and directories – so we use **.

Saved the configuration

![Capture 35 source files](https://user-images.githubusercontent.com/92916632/158699426-ee266c55-1b73-47f3-b634-d428796fce3d.PNG)



- Edited the README.MD file in my  gitHub tooling repository, commited the file 

![Capture 38 edited read me file](https://user-images.githubusercontent.com/92916632/158704868-fef032ee-1871-4d6c-822a-fa810a03c085.PNG)


- Webhook triggered a new job but it was unsuccessful as shown in the console output  screenshot below 

Error message : persmission denied 

![Capture 36 build error](https://user-images.githubusercontent.com/92916632/158705267-67095985-80c3-4b41-8d68-fcd4aa774c64.PNG)

 
- I set up permission that allows the remote directory  /mnt/apps/ to  be readable, writeable and executeable
 
       sudo chown -R nobody: /mnt/apps
       
       sudo chmod -R 777 /mnt/apps

 ![Capture 39 change permissions](https://user-images.githubusercontent.com/92916632/158706215-ab2d0afb-048f-4fa2-b731-17af4179301d.PNG)


- Ran a new build and it was successful 
 
 ![Capture 40 successful build](https://user-images.githubusercontent.com/92916632/158706702-58bdceac-92c7-4f6a-8206-931bb4663a77.PNG)


 

- To check that the files in /mnt/apps have been updated, i connected via SSH to the NFS server and checked the contents of /mnt/apps directory  
 
         cd /mnt/apps
         
          ll 
          
 ![Capture 41 check the contents of remote directory](https://user-images.githubusercontent.com/92916632/158707654-347623af-f2eb-4ba2-a9b1-a84934903c73.PNG)

      
      
- Also checked the contents of the README.MD file to confirm the changes i had previously made in my gitHub tooling repository
      
        cat /mnt/apps/README.md
        
  
![Capture 42 checked the contents of readme](https://user-images.githubusercontent.com/92916632/158707830-0c4f495f-7264-4a57-82ea-a88536cc7302.PNG)


 
 














 
 






