
# Ansible configuration management Automate project 7 TO 10

Step 1 INSTALLED AND CONFIGURED ANSIBLE ON EC2 INSTANCE

- Updated Name tag on my Jenkins EC2 Instance to Jenkins-Ansible. This server will be used run playbooks.

- In my GitHub account, i created a new repository and named it ansible-config-mgt.

![Capture 1 create a new repository](https://user-images.githubusercontent.com/92916632/153218521-f116a9ce-1bad-48d8-a578-f30000716256.PNG)


- Installed ansible 

        sudo apt update
    
        sudo apt install ansible
        
- Configured Jenkins build job to save my repository content every time i change it  

   - Created a new freestyle project ansible in Jenkins and pointed it to my ‘ansible-config-mgt’ repository.
  
  ![Capture 2 create freestyle project](https://user-images.githubusercontent.com/92916632/153218713-71a16132-43ba-4329-8381-0045d360ee29.PNG)


   - Configured Webhook in GitHub. Added the public IP address of jenkins-ansible server

   - set webhook to trigger ansible build. 

   - Connected my gitHub repository to jenkins by copying the github repository url and pasting it to jenkins 

![Capture 5 webhook](https://user-images.githubusercontent.com/92916632/153220096-e838f0c4-5bdc-4042-ba93-e143144a73e1.PNG)

![Capture 4 build triggers](https://user-images.githubusercontent.com/92916632/153219519-5b7ae9a3-b03a-4b1a-9a8e-36d5aa5b45f3.PNG)



![Capture 3 source code management](https://user-images.githubusercontent.com/92916632/153219433-1e30bf5f-8a06-4abe-b6ad-7c5295a7ed7e.PNG)



- Configure a Post-build job to save all (**) files

 ![Capture 6 post build actions](https://user-images.githubusercontent.com/92916632/153236160-b54013c2-3400-4f40-a1f2-f405f242821b.PNG)
 
![Capture 7 post build actions](https://user-images.githubusercontent.com/92916632/153236211-0f115a1d-eb02-4d3a-a2a3-b4a8a37fc6b2.PNG)

- Trigger Jenkins project execution only for /main branch.

 ![Capture 8  jenkins only for main branch](https://user-images.githubusercontent.com/92916632/153236574-0435a403-c78e-43f4-81cf-75c8f8f28c86.PNG)
 
 - Tested the configuration by editing the README.md file on github and verifying that builds starts automatically in Jenkins
 
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


- Created a directory and named it inventory  – it will be used to keep our hosts organised.

Within the inventory folder, created an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

![Capture 20 inventory](https://user-images.githubusercontent.com/92916632/153509809-7af718a7-ae30-4bc6-b55d-c9573fc4f414.PNG)

                               

Step 4 – Set up an Ansible Inventory

   
   - Updated the inventory/dev.yml file with this snippet of code:

         [nfs]
         <NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

         [webservers]
         <Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
         <Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

          [db]
          <Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

          [lb]
          <Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
          
 ![Capture 34 edit dev yml file](https://user-images.githubusercontent.com/92916632/154165730-ea42f084-759a-4c0b-8b06-271b0dcfa4b1.PNG)

    
   
   
   
   Step 5 – Created a Common Playbook
   
    
 Updated the playbooks/common.yml file with following code:
           
   -  Edited the common.yml file with the following configuration
    
    ---
    - name: Update web, NFS and DB servers
    hosts: webservers, nfs, db, lb
    remote_user: ec2-user
    become: yes
    become_user: root
    tasks:
      - name: ensure wireshark is at latest version
          yum:
          name: wireshark
          state: latest         
  
    - name: update LB server
       hosts: lb
       remote_user: ubuntu
       become: yes
       become_user: root
    tasks:
      - name: Update apt repo
        apt: 
          update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
        
        
 ![Capture 33 edit common file](https://user-images.githubusercontent.com/92916632/154163633-e3f5eaf4-c329-44b2-b3fc-7ef80ee48313.PNG)

   
   
   Step 6 – Update GIT with the latest code
   
   Committed the codes into gitbub 
   
   
   - Checked git status
   
              git status
              
 ![Capture 27 git status](https://user-images.githubusercontent.com/92916632/154054425-a42bc706-137d-4701-9f39-7480409929d3.PNG)
 
  
 -  Added all untracked files that need to be comitted 
  
               git add .
 ![Capture 28 git add](https://user-images.githubusercontent.com/92916632/154055284-9f664d0e-ce26-46cc-9c14-fceb4d39a698.PNG)

               
  - Added commit message 
              
              git commit -m "commit message"
              
  ![Capture 29 git commit](https://user-images.githubusercontent.com/92916632/154059299-5c43b4ca-0944-40a8-9c98-1249cd2ac48a.PNG)
  
  
  - Push changes to the remote branch 
   
              git push origin prj-11
              
   ![Capture 30 git push origin](https://user-images.githubusercontent.com/92916632/154062899-6469b90b-c693-40d8-801b-38864860bca1.PNG)


  - Created a pull request 

    ![Capture 20 pull request](https://user-images.githubusercontent.com/92916632/154063339-508872b4-f7b4-4355-9b0a-6bfd640a6cfb.PNG)
    
    
    
  - Merge pull request 

    ![Capture 23 merge pull request](https://user-images.githubusercontent.com/92916632/154066610-8e78c014-93dc-43a8-9b9e-7a618c6f32f0.PNG)
    
    ![Capture 24 pull request successfully merged](https://user-images.githubusercontent.com/92916632/154067496-4398ae76-a9bd-4eb8-b870-e32c6bbccf7b.PNG)
    
 -  Checked out to main branch 
    
                git checkout main 
                
    ![Capture 31 git checkout main](https://user-images.githubusercontent.com/92916632/154110450-2d0cdcd0-12c0-4016-bbd4-5fa41159393b.PNG)
    
    
   -  Git pull to update what we have in project-11 branch to the main branch 
  
             git pull 

   ![Capture 32 git pull](https://user-images.githubusercontent.com/92916632/154112693-d39f6aeb-234d-434b-89fe-c806705a266c.PNG)
   
   - Verified that Jenkins has automatically saved all the files(built artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory
    on Jenkins-Ansible server.
   
   ![Capture 25 build successful on jenkins](https://user-images.githubusercontent.com/92916632/154164220-b259791e-0c81-42a5-9c68-49b5f15cb94f.PNG)
   
   ![Capture 26 verify jenkins build](https://user-images.githubusercontent.com/92916632/154164251-8a7efed8-df0c-4b77-bf6d-91fdba2b7e0a.PNG)
   
   ![Capture 35 verified jenkins server](https://user-images.githubusercontent.com/92916632/154167354-a46098e3-0ef3-435d-9911-a0b2383ac225.PNG)

   ![Capture 36 jenkins server](https://user-images.githubusercontent.com/92916632/154167403-fb5205aa-211c-447f-97b9-6c798f3482b0.PNG)
   
   
   
   Step 7   Installed OpenSSH on windows 
   
 - To install OpenSSH using PowerShell, i ran PowerShell as an Administrator
   
   - Installed OpenSSH using PowerShell with the follwing command 
   
          Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
          
  ![Capture 1](https://user-images.githubusercontent.com/92916632/157339524-591906cc-a750-4063-9faa-400eef437409.PNG)

          
          
   - Installed the OpenSSH Client
         
           Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
          
  ![Capture 2](https://user-images.githubusercontent.com/92916632/157345372-2dcef742-631a-4a23-8123-865854ce6e48.PNG)



          
 -  Installed the OpenSSH Server
            
          Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
          
 ![Capture 3](https://user-images.githubusercontent.com/92916632/157345616-f58a14d0-549e-4732-af3f-2870dc7dda38.PNG)

 


          
          
-  Started the sshd service
          
           Start-Service sshd
           
           Set-Service -Name sshd -StartupType 'Automatic' 
           
 

 
   - Set the sshd service to be started automatically
 
            Get-Service -Name sshd | Set-Service -StartupType Automatic
            
  -  Started the sshd service
    
            Start-Service sshd
            
            
          
        
-  Configured it to be automatically started 
  
        
         Get-Service ssh-agent | Set-Service -StartupType Automatic
         
        
- Started the service

        
        Start-Service ssh-agent
        
 
- Added my pem key into ssh-agent. Added the path to the pem key
 
        ssh-add "C:\Users\USER\Downloads\richard-ec2.pem"
        
![Capture path to pem key](https://user-images.githubusercontent.com/92916632/157344792-5304dc49-3781-4ed2-a7e6-5144578f90c7.PNG)

          



Step 8  Run first Ansible test


- Confirm that pem key has been added 

        ssh-add -l
        
![Capture confirm pem key added](https://user-images.githubusercontent.com/92916632/157711988-4c75218d-2508-4904-93eb-e02718ed2f95.PNG)

- Ran eval ssh agent 

       eval `ssh-agent -s`
 
        

- From the host server( ansible-jenkins), I SSH into NFS server to verify that ansible server can access other servers using the same pem key

  ![image](https://user-images.githubusercontent.com/92916632/157560534-ff02867b-7d17-4251-ac65-188a94abbb59.png)




-  Edited and saved the SSH config file as follow: 
 
 
       # The bastion host
       Host 18.218.47.55
       HostName 18.218.47.55
       ForwardAgent yes
       User ubuntu

       Host Bastion
       HostName 18.218.47.55
       User ubuntu
       IdentityFile C:/Users/USER/Downloads/richard-ec2.pem
       ForwardAgent yes
       
  ![Capture 4 ssh config file](https://user-images.githubusercontent.com/92916632/157764981-7fdcc5cc-0d9b-4536-bc86-e262facd387a.PNG)

       
       
     *   Host - public IP address of host server(jenkins-Ansible)
       
      * Hostname - public IP address of host server(jenkins-Ansible)
 
        
        
 - Connected to the host server (Jenkins-Ansible)  on VS code. 
 
 
 - To ensure that ansible does not check my host key when connecting i ran : 
 
 
        export $ANSIBLE_HOST_KEY_CHECKING=False
 
 
 
- Ran ansible-playbook command on VS code 
 
 
      ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/Inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build-   
      
      number>/archive/playbooks/common.yml
      
 
 ![SC 1 run ansible plabbok](https://user-images.githubusercontent.com/92916632/157726100-788142b7-598f-4410-a02c-ac1777875930.PNG)
 ![SC 2 run ansible playbook](https://user-images.githubusercontent.com/92916632/157726197-5e3954f6-b835-420c-b702-96e5d591c3c5.PNG)


      
      
- Logged in to all the servers to verify that wireshark has been installed 

          wireshark --version
          
   ![Capture wireshark version nfs server](https://user-images.githubusercontent.com/92916632/157557905-81abf024-8404-4d3f-9d7b-dac463d5c0c1.PNG)
   
   ![Capture wireshark version wb server](https://user-images.githubusercontent.com/92916632/157561722-c22184e2-d07e-4f0b-9139-33e282f50e9d.PNG)


        
        
        
 






 
              
     
   
   
   
 








