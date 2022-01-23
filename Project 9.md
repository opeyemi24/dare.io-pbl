
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
 

1. Enabled webhooks in my GitHub repository settings

![Capture 7 add webhooks](https://user-images.githubusercontent.com/92916632/150678029-a4cd0984-fd37-4123-91fd-f87da62285c5.PNG)


Added webhooks. Added jenkins server public IP address


![Capture 8 add web hooks](https://user-images.githubusercontent.com/92916632/150677953-59b7c071-e599-4fb5-ad76-20583c8af88a.PNG)




2. From Jenkins web console, clicked "New Item" and created a "Freestyle project"

![Capture 9 free style project](https://user-images.githubusercontent.com/92916632/150678616-b0b4abd1-b222-42c4-88ff-c4e05dde96fa.PNG)




 
 






