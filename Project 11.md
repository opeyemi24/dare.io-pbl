
# Ansible configuration management Automate project 7 TO 10

Step 1 INSTALLED AND CONFIGURED ANSIBLE ON EC2 INSTANCE

- Updated Name tag on my Jenkins EC2 Instance to Jenkins-Ansible. This server will be used run playbooks.

- In my GitHub account, i created a new repository and named it ansible-config-mgt.

![Capture 1 create a new repository](https://user-images.githubusercontent.com/92916632/153218521-f116a9ce-1bad-48d8-a578-f30000716256.PNG)


- Installed ansible 

        sudo apt update
    
        sudo apt install ansible
        
- Configured Jenkins build job to save my repository content every time i change it  

   - Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.
  
  ![Capture 2 create freestyle project](https://user-images.githubusercontent.com/92916632/153218713-71a16132-43ba-4329-8381-0045d360ee29.PNG)


   - Configure Webhook in GitHub and set webhook to trigger ansible build. 

![Capture 5 webhook](https://user-images.githubusercontent.com/92916632/153220096-e838f0c4-5bdc-4042-ba93-e143144a73e1.PNG)


![Capture 3 source code management](https://user-images.githubusercontent.com/92916632/153219433-1e30bf5f-8a06-4abe-b6ad-7c5295a7ed7e.PNG)

![Capture 4 build triggers](https://user-images.githubusercontent.com/92916632/153219519-5b7ae9a3-b03a-4b1a-9a8e-36d5aa5b45f3.PNG)

- Configure a Post-build job to save all (**) files

 ![Capture 6 post build actions](https://user-images.githubusercontent.com/92916632/153236160-b54013c2-3400-4f40-a1f2-f405f242821b.PNG)
 
![Capture 7 post build actions](https://user-images.githubusercontent.com/92916632/153236211-0f115a1d-eb02-4d3a-a2a3-b4a8a37fc6b2.PNG)

- Trigger Jenkins project execution only for /main branch.

 ![Capture 8  jenkins only for main branch](https://user-images.githubusercontent.com/92916632/153236574-0435a403-c78e-43f4-81cf-75c8f8f28c86.PNG)
 
 - Tested the configuration by editing the README.MD file on github and verifying that builds starts automatically in Jenkins
 
 ![Capture 10 test successful](https://user-images.githubusercontent.com/92916632/153239920-ad0e38ea-8ea3-46aa-be8b-a3df21c4684e.PNG)

 
 - Verified that jenkins saves the files (built artifacts) in the following folder

       sudo ls /var/lib/jenkins/jobs/ansible/builds

       sudo ls /var/lib/jenkins/jobs/ansible/builds/<build number>/archive/
     
     
![Capture 11 confirm the contents of artifacts](https://user-images.githubusercontent.com/92916632/153241512-43023eec-319a-4c00-af45-87af7d1b2ea3.PNG)


Step 2 – Prepared my  development environment using Visual Studio Code

  - Installed Visual Studio Code from https://code.visualstudio.com/download

  -  Installed visual studio code remote development extension pack 
    
  -  Cloned the previously created repository(ansible-config-mgt) from github to VS code
    
![Capture 12 cloning](https://user-images.githubusercontent.com/92916632/153503749-3bfe1399-cfee-4a4a-8f62-7c2953ac8169.PNG)

   
- Checked the git branch
 
         git branch 
         
   ![Capture 14 checked git branch](https://user-images.githubusercontent.com/92916632/153503487-c5118cf4-4e39-49b7-9159-d481d22026a3.PNG)


     
- Created a new branch in my ansible-config-mgt repository and named it  prj-11
    
           git checkout -b prj-11
           
 ![Capture 15 created a new branch](https://user-images.githubusercontent.com/92916632/153505355-1b99c5bd-748e-4e40-af7f-ec037a900204.PNG)


- Verified the newly created branch 

           git status 

![Capture 16 checked the new branch](https://user-images.githubusercontent.com/92916632/153506060-1d2e3d35-6720-4f49-bc67-9b5e2c55a6ad.PNG)


- Created a directory and named it playbooks – it will be used to store all playbook files.

          mkdir playbooks

![Capture 17 create play books directory](https://user-images.githubusercontent.com/92916632/153507591-22ff0a20-38e9-42c8-9c4c-13710fb5efad.PNG)

- Within the playbooks folder, created my first playbook and named it common.yml

![Capture 19 created common file](https://user-images.githubusercontent.com/92916632/153509185-9bb8bc74-7158-4068-8533-537e96dd9727.PNG)


- Created a directory and named it inventory – it will be used to keep our hosts organised.

Within the inventory folder, created an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

![Capture 20 inventory](https://user-images.githubusercontent.com/92916632/153509809-7af718a7-ae30-4bc6-b55d-c9573fc4f414.PNG)




           
           
           
           
 Step 4 – Set up an Ansible Inventory

 








