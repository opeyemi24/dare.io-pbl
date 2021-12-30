
 # DEVOPS TOOLING WEBSITE SOLUTION
 
 In this project i implemented a solution that consists of the following components:
 
 1. infrastructure: AWS
 
 2. Webserver Linux: Red Hat Enterprise Linux 8
 
3. Database Server: Ubuntu 20.04 + MySQL

4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server

5. Programming Language: PHP

6. Code Repository: GitHub

STEP 1 : Prepared the webserver

1. Launched 4 EC2 instances. 3 instances will serve as 'webserver' while 1 instance will serve as 'storage server' all using Redhat OS

![Capture 1 -  4 instances](https://user-images.githubusercontent.com/92916632/147556253-5a893d3b-d388-4179-b96b-db9df2f3a21e.PNG)



2. Launched 1 EC2 instance to serve as the 'database server' using ubuntu OS

![Capture 2 DB SERVER](https://user-images.githubusercontent.com/92916632/147556339-b969b430-c6fd-4664-9a29-72d8f618af7b.PNG)

3. Created 3 volumes in the same availability zone as my nfs server each of 10G 
   
   Screenshot showing the creation of volumes 
   
   ![Capture 3 created volumes](https://user-images.githubusercontent.com/92916632/147659725-06064768-c4d3-43c2-8cb9-b12af86d0c7d.PNG)
   
   

4. Attached the 3 volumes one after the other to my nfs server EC2 instance

Screenshot showing the attachment of volumes

![Capture 4 attach vol 1](https://user-images.githubusercontent.com/92916632/147660159-acfa3fbd-30c1-4149-a053-ec0f3b39586f.PNG)

![Capture 5 attach vol 2](https://user-images.githubusercontent.com/92916632/147660288-28321336-cf3c-4a6e-90ad-3e4e3cbb93b3.PNG)

![Capture 5 attach vol 3](https://user-images.githubusercontent.com/92916632/147660408-18ef0d72-91bf-49fa-99c3-ed3a563b101a.PNG)



5.  To confirm that the newly created devices have been attached, i ran the command : lsblk 

The names of the the newly created block devices are xvdf, xvdg, xvdh as shown in the screenshot below

![Capture 6 lsblk](https://user-images.githubusercontent.com/92916632/147660736-5412b6f7-d3f8-431b-8f72-f920647a1d45.PNG)

6.  I used gdisk utility to create a single partition on each of the 3 disks. To create partition on the first disk, i ran the command below:

            sudo gdisk /dev/xvdf
            
    
 Took the following steps :
 
 Typed n for new partition

Partition number : 1

First sector (size which i want the disk used for) : clicked enter to use the entire disk

Last sector : clicked enter to use entire disk

Hex code or Guid = 8300 to change partition type to Linux LVM

Command ( ? for help) : Typed p to check check the configuration

Command ( ? for help) : Typed w to write

Do you want to proceed (Y/N) : Yes

Screenshot showing the creation of partition on disk xdvf
![Capture 7 xdf](https://user-images.githubusercontent.com/92916632/147661161-0d47865f-5924-4e5f-8d84-5c199ffecdb1.PNG)
![Capture 7b xdf](https://user-images.githubusercontent.com/92916632/147661301-afc9d60e-61d2-4f97-a7de-3438d49b8836.PNG)

7.  For the creation of partition on the second disk, i ran the following command:

          sudo gdisk /dev/xvdg 
          

Followed the above named procedure which is also illustrated in the screenshot below
![Capture 8 xdg](https://user-images.githubusercontent.com/92916632/147661753-545e3028-d9f8-4813-a4d1-db8ef775ef87.PNG)


8. For the creation of partition on the third disk, i ran the following command :

         sudo gdisk /dev/xvdh
 
   Followed the above named procedure which is also illustrated in the screenshot below 
  ![Capture 9 xdh](https://user-images.githubusercontent.com/92916632/147662065-e2c4fb2e-8878-436e-aee5-915b75d2ab43.PNG)
  
  
9.  To view the newly configured partition on each of the 3 disks. i ran : lsblk

    ![Capture 10 lsblk](https://user-images.githubusercontent.com/92916632/147662378-7e5aac69-3971-4325-951c-ef97a9c01584.PNG)
    

10.  Installed lvm2 package by running the following command :

         sudo yum install lvm2 -y
         
11. To check for the available partitions i ran : 
   
          sudo lvmdiskscan
          
    ![Capture 12 lvm diskscan](https://user-images.githubusercontent.com/92916632/147685264-cfe562e1-b12d-4d40-aef7-9ab2f63d8a78.PNG)
    
    
12.  I marked each of the 3 disks as physical volume with the command :

          sudo pvcreate /dev/xvdf1
          
          sudo pvcreate /dev/xvdg1
          
          sudo pvcreate /dev/xvdh1
          
     ![Capture 13 pvcreate](https://user-images.githubusercontent.com/92916632/147691068-3eff5563-adb3-4d52-b703-e554f2f0407f.PNG)
     

13.   Verified that my physical volume has been created by running the following command :

           sudo pvs
           
      ![Capture 15 sudo pvs](https://user-images.githubusercontent.com/92916632/147691253-375eabe6-8031-47f7-a938-d69ceda4ec77.PNG)
      
14.   Used vgcreate utility to add all 3 PVs to a volume group (VG). I Named the VG webdata-vg

            sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
            
      ![Capture 18 created volume group](https://user-images.githubusercontent.com/92916632/147691604-867f9002-ae1c-4dec-a857-ef1d38421399.PNG)
      
 15.   Verified that my volume group has been created successfully by running the following command :

             sudo vgs
             
       ![Capture 19 sudo vgs](https://user-images.githubusercontent.com/92916632/147691969-87308508-e9fe-46eb-9bcf-7503d409fb34.PNG)
       
 16.   Used lvcreate utility to create 3 logical volumes. lv-apps, lv-logs, lv-opt. I allocated 9 GIG to each of them

           sudo lvcreate -n lv-apps -L 9G webdata-vg
           
           sudo lvcreate -n lv-logs -L 9G webdata-vg
           
           sudo lvcreate -n lv-opt  -L 9G webdata-vg
           
      
 ![Capture 20 created logical volumes](https://user-images.githubusercontent.com/92916632/147692757-4af7950b-5c71-422f-b86a-6d5c1bcacf30.PNG)
 
 17.  Verified that my logical volume has been created by running : sudo lvs

      ![Capture 21 sudo lvs](https://user-images.githubusercontent.com/92916632/147693659-f1e1aaf9-db04-40e6-baf3-8880e869c37b.PNG)
      
 18.   Verified the set up 

             sudo lsblk
             
       ![Capture 22 sudo lsblk](https://user-images.githubusercontent.com/92916632/147693825-e49ba230-3ac0-47a3-814e-a27303075eb9.PNG)
       
       
 19.  I used mkfs.xfs to format the logical volumes with xfs filesystem

            sudo mkfs -t xfs /dev/webdata-vg/lv-apps 
    
            sudo mkfs -t xfs /dev/webdata-vg/lv-logs
            
            sudo mkfs -t xfs /dev/webdata-vg/lv-opt
            
      ![Capture 23 formating the disk](https://user-images.githubusercontent.com/92916632/147699778-d7e97d5d-9247-4d0b-b7c4-8d1bedd756bc.PNG)
      
      
20.   Created mounting points on /mnt directory for the logical volumes as follows : 

            sudo mkdir /mnt/apps
            
            sudo mkdir /mnt/logs
            
            sudo mkdir /mnt/opt
            
            
21.    Mounted lv-apps on /mnt/apps – To be used by webservers

       Mounted lv-logs on /mnt/logs – To be used by webserver logs
       
       Mounted lv-opt on /mnt/opt – To be used by Jenkins server in Project 8
       
  ![Capture 25 mounted](https://user-images.githubusercontent.com/92916632/147758993-69a9e36e-8304-4348-83ce-291b25b8c606.PNG)


22. Installed NFS server & configured it to start on reboot

           sudo yum -y update
           
           sudo yum install nfs-utils -y
           
           sudo systemctl start nfs-server.service
           
           sudo systemctl enable nfs-server.service
           
           sudo systemctl status nfs-server.service
           
           
 23.  set up permission that allows our web servers to read, write and execute files on NFS:

            sudo chown -R nobody: /mnt/apps
            
            sudo chown -R nobody: /mnt/logs
            
            sudo chown -R nobody: /mnt/opt
            
            sudo chmod -R 777 /mnt/apps

            sudo chmod -R 777 /mnt/logs

            sudo chmod -R 777 /mnt/opt
 
![Capture 27  chown](https://user-images.githubusercontent.com/92916632/147758051-3ea3326f-5b7e-4ab9-bdb2-6c9ebfc5c360.PNG)

24.  Re-started the NFS server

           sudo systemctl restart nfs-server.service
           
 ![Capture 28 restart nfs service](https://user-images.githubusercontent.com/92916632/147760100-4d0ed2a2-ad1c-49ce-a00d-4415c5c2aae3.PNG)
 
 
 25. Configured access to NFS for clients within the same subnet

           sudo vi /etc/exports

           /mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
          
          /mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

          /mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash) 
         
          Esc + :wq!
          
   ![Capture 29](https://user-images.githubusercontent.com/92916632/147771300-7b0b68fd-be51-4383-9a43-f1487e1f900c.PNG)

          
          
         sudo exportfs -arv
         
   ![Capture 30 exporting](https://user-images.githubusercontent.com/92916632/147771341-4381b2fa-1651-4119-98d3-bcffe096ed26.PNG)
   

  26.  Checked which port is used by NFS and opened it using Security Groups (add new Inbound Rule)

         ![image](https://user-images.githubusercontent.com/92916632/147771641-ce7ddccf-6475-40f3-9740-cb4ae7afa3d1.png)
         
  
  
  In order for NFS server to be accessible from my client, i also opened the following ports: NFS 2049, TCP 111, UDP 111, UDP 2049
         
  ![Capture 32 inbound rules](https://user-images.githubusercontent.com/92916632/147778876-07be0a71-bf58-47fc-8218-89a48103b319.PNG)
  
  
  
  
  STEP 2 : CONFIGURED THE DATABASE SERVER 
  
 
 1.    Installed mysql server 
  
            sudo apt udate 

            sudo apt install mysql-server -y
            
  
  2.  Created a database and named it tooling 

              sudo mysql
             
             create database tooling;
             
  3.    Created a database user and name it webaccess

        


 
            

         
         
         
  

         
         

     









