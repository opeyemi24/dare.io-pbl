
# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. 

# Continous Integration pipeline for tooling website 

Step 1 – Install Jenkins server

1. Created an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"

2. Installed JDK (since Jenkins is a Java-based application)

       sudo apt update
       
       sudo apt install default-jdk-headless
       
3. Installed Jenkins

       wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
       
       sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
       
          /etc/apt/sources.list.d/jenkins.list'
             
       sudo apt update
       
       sudo apt-get install jenkins
       
  Made sure Jenkins is up and running
  
        sudo systemctl status jenkins
        
 4. Opened TCP port 8080. By default Jenkins server uses TCP port 8080




5. Performed initial Jenkins setup


From my browser, i accessed :

          http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
  

  I was prompted to provide a default admin password
  
  
  Retrieved it from my server:
  
         sudo cat /var/lib/jenkins/secrets/initialAdminPassword
         

 
 Was prompted to choose plugings to install – I chose suggested plugins.
 
 






