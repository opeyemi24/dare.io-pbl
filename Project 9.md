
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

       
  Made sure Jenkins is up and running
  
        sudo systemctl status jenkins
        
![Capture 2 status jenkins](https://user-images.githubusercontent.com/92916632/150677178-4c8cb1c3-a391-4f20-aac2-4335f8ff8f1f.PNG)

        
 4. Opened TCP port 8080. By default Jenkins server uses TCP port 8080




5. Performed initial Jenkins setup


From my browser, i accessed :

          http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
  

  I was prompted to provide a default admin password
  
 ![Capture jenkins home page](https://user-images.githubusercontent.com/92916632/150677479-0e0167dd-1075-4331-8edb-6d1e4cdba5bc.PNG) 
  

  
  Retrieved it from my server:
  
         sudo cat /var/lib/jenkins/secrets/initialAdminPassword

![Capture 4 initial admin password](https://user-images.githubusercontent.com/92916632/150677646-83163612-486f-462c-9acb-3be788a74c92.PNG)

![Capture jenkins home page 2](https://user-images.githubusercontent.com/92916632/150677814-2cc5cf1c-9fa2-4592-9e4f-f548fa75cb0a.PNG)


 
 Was prompted to choose plugings to install – I chose suggested plugins.
 
 ![Capture 3 suggested plug ins](https://user-images.githubusercontent.com/92916632/150677349-00e378d9-4b74-476e-bdd0-845612c45ac6.PNG)

 
 
 
 Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks
 

1. Enabled webhooks in my tooling gitHub repository settings

![Capture 7 add webhooks](https://user-images.githubusercontent.com/92916632/150678029-a4cd0984-fd37-4123-91fd-f87da62285c5.PNG)


Added webhooks.   Added jenkins server public IP address


![Capture 8 add web hooks](https://user-images.githubusercontent.com/92916632/150677953-59b7c071-e599-4fb5-ad76-20583c8af88a.PNG)




2. From Jenkins web console, clicked "New Item" and created a "Freestyle project"

![Capture 9 free style project](https://user-images.githubusercontent.com/92916632/150678616-b0b4abd1-b222-42c4-88ff-c4e05dde96fa.PNG)



To connect my GitHub repository, you will need to provide its URL, you can copy from the repository itself

![Capture 10 repository url](https://user-images.githubusercontent.com/92916632/157853240-7677e61a-a73e-4d05-9bd2-4186779e8f81.PNG)


In configuration of your Jenkins freestyle project choose Git repository, provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

![Capture add credentials and save](https://user-images.githubusercontent.com/92916632/157853980-06b55292-cd6b-401f-a5b1-98d19c17ec44.PNG)


![Capture configure global security](https://user-images.githubusercontent.com/92916632/157860419-6ee9223d-2470-4164-a16a-1d32464de7d6.PNG)

Enabled proxy compatibility and saved 

![Capture enable proxy compabitibility](https://user-images.githubusercontent.com/92916632/157860460-c53364a8-8b8e-4fa0-a78f-dcae7bcc8fbe.PNG)


Add credentials  and save 
 
![Capture add credentials and save](https://user-images.githubusercontent.com/92916632/157861647-da53dd4a-b8a6-4d4e-bdeb-9c679c25c933.PNG)


![Capture add credentials](https://user-images.githubusercontent.com/92916632/157861699-aec784b9-b887-4503-87d4-cd98a2f4d21a.PNG)

selected the credentials created and saved

![Capture credentials created](https://user-images.githubusercontent.com/92916632/157862628-e654bb52-b913-42f0-b0ee-84c59d488954.PNG)


Ran a build manaually by clicking build now 


![Capture build noww](https://user-images.githubusercontent.com/92916632/157863914-406e550b-e85c-4f82-805a-0b9ef6d740ff.PNG)


Verified the console output 

![Capture build- console output](https://user-images.githubusercontent.com/92916632/157863095-fa5abaa6-79c1-4a18-ba2a-71ecb9dc777a.PNG)


Step 3 Click "Configure" your job/project and add these two configurations

    Configure triggering the job from GitHub webhook:

  
  ![Capture build triggers](https://user-images.githubusercontent.com/92916632/157864567-657acc45-78c5-425c-b682-abd7cc22986a.PNG)
  
  
   Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".
   
 Clicked post build actions, archive the artifacts, added **  and saved 
 
    
![Capture archive the artifacts](https://user-images.githubusercontent.com/92916632/157866129-4f8fda6f-74f5-4ce8-8eb4-fe4324647891.PNG)


make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.


![Capture added new line to readme file](https://user-images.githubusercontent.com/92916632/157866907-b44444e5-8c28-449c-93cc-22a703365c60.PNG)

A new build was launched automatically (by webhook) and we see its results – artifacts, saved on Jenkins server.


![Capture console output  2a](https://user-images.githubusercontent.com/92916632/157868075-3d9fc171-d003-42dc-8ce5-6ec270d05f06.PNG)


Console output

![Capture console output 2b](https://user-images.githubusercontent.com/92916632/157868250-cba78f83-54aa-4a63-963b-aa7fde5c9ec2.PNG)

Screenshot showing the artifacts generated as a result of the latest build 


![Capture built artifacts](https://user-images.githubusercontent.com/92916632/157869084-274684e0-d19c-49e8-9de4-91a3682ec26f.PNG)




 CONFIGURE JENKINS TO COPY FILES TO NFS SERVER VIA SSH
 
 On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.
 
 Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:















 
 






