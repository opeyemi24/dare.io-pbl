
# ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)


Step 1 – Jenkins job enhancement


 1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

            sudo mkdir /home/ubuntu/ansible-config-artifact
            
 2. Change permissions to this directory, so Jenkins could save files there 

           sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact
           
 
  3. Created an explore page for the newly created directory. From VS code, clicked open, open folder, chose the newly created directory.

![Capture open folder](https://user-images.githubusercontent.com/92916632/169154997-4209225b-c86e-443b-ac0b-8528f7334205.PNG)

  
  
  
             
 4. From Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

  ![image](https://user-images.githubusercontent.com/92916632/161092590-5d869d41-9b81-401c-aa33-5ac75ef91f64.png)


4. Create a new Freestyle project and name it save_artifacts

 ![Capture 3 free style project](https://user-images.githubusercontent.com/92916632/161128801-7b1ad998-95cd-4521-9f4b-529062491418.PNG)


5. Configure its job to run after the ansible jenkins job is done:

![image](https://user-images.githubusercontent.com/92916632/161136118-111eea08-446c-4f96-8f0f-d3f1980f010d.png)


6.The new project will be configured to copy artifacts from the ansible jenkins utilized in project 9 to the newly created directory: 

![Capture 4a](https://user-images.githubusercontent.com/92916632/161140301-07ce1485-6ef4-40b5-bb98-7ce1df7f1ee6.PNG)

![Capture 4b](https://user-images.githubusercontent.com/92916632/161140358-0e789a2b-dc86-4ec5-b5e8-14fae05375fc.PNG)


Test your set up by making some change in README.MD file inside your ansible-config-mgt repository

![Capture testing config](https://user-images.githubusercontent.com/92916632/161156214-0f98a8a2-aa7d-49ba-bd58-1e524abbea11.PNG)





Verified that build ran successfully on both jobs i.e ansible & save_artifacts


![Capture 5a](https://user-images.githubusercontent.com/92916632/161158876-b1c6872b-33dc-44ce-852d-1694f90bec31.PNG)


![Capture 5  console output](https://user-images.githubusercontent.com/92916632/161157413-45060c55-bf81-4f2c-b6ea-ea1fe7723a5e.PNG) 

Checked the /home/ubuntu/ansible-config-artifact directory to see if the files are updated 

![Capture 6 testing read me](https://user-images.githubusercontent.com/92916632/161159622-ce39683b-b7e7-45d2-ba3d-c0c53a3024bb.PNG)


The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory.



STEP 2 : Refactor Ansible code by importing other playbooks into site.yml



1.  Within playbooks folder, created a new file and name it site.yml 
         
 
 ![Create a new file called site yml](https://user-images.githubusercontent.com/92916632/169409393-fc72c7ff-d94e-480a-ac5f-20b15ad9c170.PNG)
 
  
  
         
2. Created a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored.
     
     
3.  Moved common.yml file into the newly created static-assignments folder.
 
![Capture static assignments](https://user-images.githubusercontent.com/92916632/169410461-4299afb6-0764-4f94-80e3-543432a77eb2.PNG)

     
     
4. Inside site.yml file, imported common.yml playbook.
 
```
--- 
- hosts: all
- import_playbook: ../static-assignments/common.yml

```

![Capture site](https://user-images.githubusercontent.com/92916632/169605665-5106c11b-5631-4d3f-9b71-6ef8ad73d5d7.PNG)


Folder structure looks like this : 

![Capture folder structure](https://user-images.githubusercontent.com/92916632/169607776-c120aae1-7afd-4320-840a-a672281bdbc2.PNG)


Created another playbook under static-assignments and named it common-del.yml. The purpose of this is to remove previously installed wireshark.

![Capture del playbook](https://user-images.githubusercontent.com/92916632/169642014-0dd96247-5114-4e9d-9229-1ba13a60899f.PNG)


Included the script below in common-del.yml

```

---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    yum:
      name: wireshark
      state: removed

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    apt:
      name: wireshark-qt
      state: absent
      autoremove: yes
      purge: yes
      autoclean: yes     
 ```

![Capture delete wireshark](https://user-images.githubusercontent.com/92916632/169642532-2aebcfee-c32d-47d7-ad3c-d60fe239094d.PNG)

Edited site.yml to point to the common-del.yml file instead of common.yml

```
--- 
- hosts: all
- import_playbook: ../static-assignments/common-del.yml

``` 

![Capture update common-del](https://user-images.githubusercontent.com/92916632/169644103-48cd479a-0171-49aa-b34c-54e65aaad42a.PNG)


Updated the changes on vs code : clicked source control, update, commit and then push 

A new build was triggered. Confirmed the changes on github

![Capture changes in git hub](https://user-images.githubusercontent.com/92916632/169914281-54707635-d2eb-4bfe-8298-a34bb8bae614.PNG)


Switched to ansible-config artifact to confirm that changes made were effected on artifact. clicked new folder, then selected the artifact-config directory

![Capture ansibe config artifact](https://user-images.githubusercontent.com/92916632/169918706-74de90b2-c449-42e3-a45f-0b0156846999.PNG)

 
 To ensure that ansible does not check my host key when connecting i ran :
 
      export $ANSIBLE_HOST_KEY_CHECKING=False
 

From my ansible server, I ran Inventory/dev.yml against Playbooks/site.yml

    ansible-playbook -i /home/ubuntu/ansible-config-artifact/Inventory/dev.yml /home/ubuntu/ansible-config-artifact/Playbooks/site.yml
    
    
![image](https://user-images.githubusercontent.com/92916632/170641898-1dafe06c-55ee-4231-ab8c-5d66ccdee18e.png)


Verified that wireshark has been removed on host servers

![image](https://user-images.githubusercontent.com/92916632/170643502-16cfbe45-b665-4a82-9cf4-b60d5b0c2748.png)

![Capture verified that wireshark was removedd](https://user-images.githubusercontent.com/92916632/170644798-cbc6a26c-4449-4b42-859c-594c12b9cf9b.PNG)



Step 3 – Configure UAT Webservers with a role ‘Webserver’

Launched 2 fresh EC2 instances using RHEL 8. we will use them as our uat servers. Gave them names accordingly – Web1-UAT and Web2-UAT.

![Capture web uat](https://user-images.githubusercontent.com/92916632/172461707-c12ff3bd-ea0e-4160-83e9-edb840799f7d.PNG)

 
 Created a new branch called refactor 
 
      git checkout -b refactor
   
   ![Capture refactor branch](https://user-images.githubusercontent.com/92916632/172673258-cc06e4ae-c420-4430-8962-1c39a126a556.PNG)



Created the roles directory called webserver. The roles folder structure looks like this : 

         └── webserver
    ├── README.md
    ├── defaults
    │   └── main.yml
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── tasks
    │   └── main.yml
    └── templates


![Capture roles folder](https://user-images.githubusercontent.com/92916632/172674954-f52204ee-3ec5-47e4-b62c-004e85bf13b5.PNG) 


Updated my inventory ansible-config-mgt/inventory/uat.yml file with the private IP addresses of my 2 UAT Web servers

![Capture uat webservers](https://user-images.githubusercontent.com/92916632/172702967-5ebd9055-08cb-4067-a79d-a5c25cc3881b.PNG)

  From my ansible server , I opened   /etc/ansible/ansible.cfg file, uncommented roles_path string and provided a full path to 
  my roles directory :  roles_path    = /home/ubuntu/ansible-config-artifact/roles

     sudo vi /etc/ansible/ansible.cfg
     
  ![Capture 20 roles path](https://user-images.githubusercontent.com/92916632/172706493-0d7298d8-bd50-45c5-897c-657f27e5d3cd.PNG)
  
  saved and exited using esc : wq! and enter
  
  
  From Tasks directory, and within the main.yml file, I wrote configuration tasks to do the following:
  
- Install and configure Apache (httpd service)

- Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.

- Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.

- Make sure httpd service is started
  
 
 The main.yml file consists of following tasks:
 
  ```
  
  ---
- name: install apache
  become: true
  ansible.builtin.yum:
    name: "httpd"
    state: present

- name: install git
  become: true
  ansible.builtin.yum:
    name: "git"
    state: present

- name: clone a repo
  become: true
  ansible.builtin.git:
    repo: https://github.com/opeyemi24/tooling.git
    dest: /var/www/html
    force: yes

- name: copy html content to one level up
  become: true
  command: cp -r /var/www/html/html/ /var/www/

- name: Start service httpd, if not started
  become: true
  ansible.builtin.service:
    name: httpd
    state: started

- name: recursively remove /var/www/html/html/ directory
  become: true
  ansible.builtin.file:
    path: /var/www/html/html
    state: absent
    
   ```
![Capture 25 task](https://user-images.githubusercontent.com/92916632/172719195-4bb76551-83a1-4ccb-8519-89b8d3403540.PNG)
 
 Step 4 – Reference ‘Webserver’ role
 
 Within the static-assignments folder, created a new file for uat-webservers called uat-webservers.yml .This is where we will reference the role
 
 Pasted the script below inside the uat-web servers.yml file
  
 ``` 
---
- hosts: uat-webservers
  roles:
     - webserver 
  ```
 
 ![Capture 26 Uat webserver](https://user-images.githubusercontent.com/92916632/172723522-14475957-348a-40c1-a4a1-f5d963639cfe.PNG)
 
 Since site.yml is our entry point then we need to refer uat-webservers.yml role inside site.yml
 
 So, we should have this in site.yml
 
 ```
 ---
- hosts: all
- import_playbook: ../static-assignments/common.yml

- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml
 
 ```
 ![site](https://user-images.githubusercontent.com/92916632/172735225-c2eaeade-0dfe-4e82-9910-bb6c538336f5.PNG)

 
 
Step 5 – Commit & Test
 
 Commit changes : click Source control, add commit message, commit, push 
 
 ![Capture 30 source control](https://user-images.githubusercontent.com/92916632/172728540-51b80fa2-5515-463d-99f7-23b17c1d8387.PNG)

 
 Merge with master branch on github : Compare and pull request, create pull request, merge pull request, confirm merge
 
![Capture 31 compare and pull request](https://user-images.githubusercontent.com/92916632/172730646-ea1732eb-62d4-46f8-9d4f-8d6127ba7587.PNG)
 

![Capture 32 create pull request](https://user-images.githubusercontent.com/92916632/172730812-f7c2db7a-1e21-4789-a9c7-93e2f4c28028.PNG)
 

![Capture 33 merge pull request](https://user-images.githubusercontent.com/92916632/172730860-96e9863d-0789-4054-9d0b-41448f545425.PNG)
 
 
 
 Ran the playbook against UAT Inventory to deploy the scripts to the UAT web servers:
 
 ```
 ansible-playbook -i /home/ubuntu/ansible-config-artifact/Inventory/uat.yml /home/ubuntu/ansible-config-artifact/Playbooks/site.yml
 
 ```
 
 ![Capture playbook 1](https://user-images.githubusercontent.com/92916632/172735916-feac9900-e1a2-422f-a51a-7c063b8cc502.PNG)
  ![Capture playbook 2](https://user-images.githubusercontent.com/92916632/172735946-01fce91b-48f1-44c2-8335-4755def86103.PNG)


 
 
 Navigated to the UAT webserver's Public IP and confirmed that it shows the tooling page:
 
 ![Capture tooling website](https://user-images.githubusercontent.com/92916632/172732468-f57c535a-6caf-45f0-91ff-f605e0b9f022.PNG)


 

 
 









        
 
    







