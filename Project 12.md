
# ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)


Step 1 – Jenkins job enhancement

 1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

            sudo mkdir /home/ubuntu/ansible-config-artifact
            
 2. Change permissions to this directory, so Jenkins could save files there –

           sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact
             
 3. From Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

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












