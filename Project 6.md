
# WEB SOLUTION WITH WORDPRESS

# TASK - To prepare a storage infrastructure on two Linux servers and implement a basic web solution using WordPress

# Configure storage subsystem for Web and Database servers based on Linux OS
     
# To ensure that the disks used to store files on the Linux servers were adequately partitioned and managed through  programs such as gdisk and LVM respectively.

# Install WordPress and connect it to a remote MySQL database server
 
 
 
 STEP 1 : Prepared a webserver
 
 1.  Launched an EC2 instances that will serve as 'webserver' using Redhat OS
 
 Screenshot showing the creation of EC2 instance
 ![Capture 1b](https://user-images.githubusercontent.com/92916632/146448622-190d94aa-e332-403f-857d-66ae084f22b3.PNG)


 
2.   Created 3 volumes in the same availability zone as my webserver EC2, each of 10G
 
 Screenshot showing the creation of volumes 
 
 ![Capture 2 creation of volumes](https://user-images.githubusercontent.com/92916632/146151256-7640a96f-eda4-49be-a084-f7f941a2a67c.PNG)

 
 
 3.  Attached the 3 volumes one by one to my webserver EC2 instance
 
  Screenshot showing the attachment of volumes to webserver EC2 instance
 
 ![Capture 3 attach volume 1](https://user-images.githubusercontent.com/92916632/146151490-cc439dab-300d-45a5-b013-11b7858f7c7e.PNG)
 
 ![Capture 3c attach vol 2](https://user-images.githubusercontent.com/92916632/146151846-56c51d87-8ea3-4642-93aa-7f5d828604d5.PNG)
 
 ![Capture 3d attach vol 3](https://user-images.githubusercontent.com/92916632/146151917-c0a7e6ca-95be-473c-802f-648fe0cbe818.PNG) 
 

4.  To confirm that the newly created devices have been attached, i ran the command : lsblk
 
 The names of the the newly created block devices are xvdf, xvdg, xvdh as shown in the screenshot below 
 
 ![Capture 7 lsblk command](https://user-images.githubusercontent.com/92916632/146231101-6d74c41f-099a-4345-bf8b-96d96e6b687f.PNG)

 
  All devices in Linux reside in /dev/ directory. To inspect we can run ls /dev/
  
  ![Capture 8](https://user-images.githubusercontent.com/92916632/146231535-4350eb20-ce8f-464c-8392-ddee2c1306fd.PNG)
  
  
5. To see all mounts and free space on my server, i ran df -h 
  
   Screenshot showing that the 3 newly added devices have not been mounted
  ![Capture 9](https://user-images.githubusercontent.com/92916632/146231757-f7ebeba3-f6b0-4c71-8926-d2c141cd42ab.PNG)
  
  
6. I used gdisk utility to create a single partition on each of the 3 disks. To create partition on the first disk, i ran the command below:
  
  
       sudo gdisk /dev/xvdf
  
   Took the following steps : 
  
  Typed n for new partition 
  
  Partition number : 1
  
  First sector (size which i want the disk used for) : clicked enter to use the entire disk
  
  Last sector : clicked enter to use entire disk
  
  Hex code or Guid = 8e00 to change partition type to Linux LVM 
  
  
  Command ( ? for help) : Typed p to check check the configuration
  
  Command ( ? for help) : Typed w to write 
  
  Do you want to proceed (Y/N) : Yes
  
  
   Screenshot showing the creation of partition on disk xdvf
  ![Capture capture 10](https://user-images.githubusercontent.com/92916632/146240112-d016c1ab-0f0d-4ceb-9de4-5a0cb79d0812.PNG)
  
  
 7. For the creation of partition on the second disk, i ran the following command:
  
         sudo gdisk /dev/xvdg 
  
  Followed the above named procedure which is also illustrated in the screenshot below
   ![Capture 11](https://user-images.githubusercontent.com/92916632/146241325-7b03585a-994b-47df-8a76-e805a1aa685d.PNG)
   
 8. For the creation of partition on the third disk, i ran the following command :
   
         sudo gdisk /dev/xvdh
  
  Followed the above named procedure which is also illustrated in the screenshot below
   ![Capture 12](https://user-images.githubusercontent.com/92916632/146241861-9cc04cd9-a78a-4f17-aa69-23f76f5ebc47.PNG)
  
  9. To view the newly configured partition on each of the 3 disks. i ran : lsblk
   ![Capture 13 to confirm configuration](https://user-images.githubusercontent.com/92916632/146242086-275cf978-2993-4028-b985-365d7c06a9fa.PNG)
   
 
  Physical volumes will be created on these partitons.
  
 10. Installed lvm2 package by running the following command : 
  
         sudo yum install lvm2 -y
  ![Capture 14 install lvm2 package](https://user-images.githubusercontent.com/92916632/146272224-357405da-8669-4147-b1c1-1c28c6c95502.PNG)
  
  
 11.  I marked each of the 3 disks as physical volume with the command :   
  
          sudo pvcreate /dev/xvdf1
 
          sudo pvcreate /dev/xvdg1
  
          sudo pvcreate /dev/xvdh1
  
 ![Capture 18](https://user-images.githubusercontent.com/92916632/146274626-b07ea3da-3ca2-4dce-b6ee-f87391a8de71.PNG)
  
 12.  Verified that my physical volume has been created by running the following command  : 
  
        sudo pvs
  ![Capture 15 confirm physical volume](https://user-images.githubusercontent.com/92916632/146273030-c5181b09-9c28-4bc9-8d5b-a2bb60d2d0b3.PNG)
  
 
 13.  Used vgcreate utility to add all 3 PVs to a volume group (VG).  I Named the VG webdata-vg 
  
          sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
  
  ![Capture 16 creation of volume group](https://user-images.githubusercontent.com/92916632/146273540-04c40c55-0df9-4645-986e-67a6e6dc5733.PNG)
  
 14. Verified that my Volume group has been created successfully by running the following command  : 
  
          sudo vgs
  ![Capture 17 verified that volume group has been created](https://user-images.githubusercontent.com/92916632/146274109-f8a261e5-d5e9-40bf-b824-0befa7dcbaf5.PNG)
  
 15.  Used lvcreate utility to create 2 logical volumes.  app-lv(i used half of the physical volume size) and log-lv( i used the remaining space of the pv size). 
  
  These logical volumes are the actual disks to be allocated to the servers.
  
  app-lv will be used to store data for the website, while log-lv will be used to store data for logs
  
          sudo lvcreate -n apps-lv -L 14G webdata-vg
  
          sudo lvcreate -n logs-lv -L 14G webdata-vg
  
  ![Capture 18 create logical volume](https://user-images.githubusercontent.com/92916632/146404757-c50429d9-725f-49d4-a079-9e93066936cc.PNG)
  
 16. Verified that my logical volume has been created by running : sudo lvs
  ![Capture 19 verify creation of logical volume](https://user-images.githubusercontent.com/92916632/146405492-fd81f5ff-15f3-4ef0-bd94-14544dcc593f.PNG)
  
  
 17.  Verified  set up 
  
           sudo lsblk
  
  ![Capture 20 verify the entire set up](https://user-images.githubusercontent.com/92916632/146406561-dfcdc2ae-880d-4dd8-a864-6dd7efa1325f.PNG)
  
 18.  I used mkfs.ext4 to format the logical volumes with ext4 filesystem
  
           sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
  
           sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
  
  ![Capture 23 combination of 2 file system](https://user-images.githubusercontent.com/92916632/146411537-d881ae6c-53dc-4ebd-9c0e-4405c020c4f7.PNG)


 19.  Created  /var/www/html directory to store website files
    
           sudo mkdir -p /var/www/html
   ![Capture 24](https://user-images.githubusercontent.com/92916632/146422196-61d5021e-e51f-42fe-9f8f-95d5940d9062.PNG)
   
 20.  Created /home/recovery/logs to store backup of log data
   
          sudo mkdir -p /home/recovery/logs
   ![Capture 25](https://user-images.githubusercontent.com/92916632/146422430-ece1e3cb-f301-47cb-9fe1-9aae598a9235.PNG)
   
 21.  Mounted /var/www/html on apps-lv logical volume
   
          sudo mount /dev/webdata-vg/apps-lv /var/www/html/
   ![Capture 27](https://user-images.githubusercontent.com/92916632/146423741-2f0e5fd4-fdee-427e-92bd-568028ebbaec.PNG)
   
 22.  Used rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)
   
          sudo rsync -av /var/log/. /home/recovery/logs/
   
   ![Capture 28](https://user-images.githubusercontent.com/92916632/146424483-5aed6f32-ca7b-472e-8609-b1ad42bc0c1c.PNG) 
   
 23.  Mounted /var/log on logs-lv logical volume. (All the existing data on /var/log will be deleted. That is why step 22 above is very important)
   
          sudo mount /dev/webdata-vg/logs-lv /var/log
   
  ![Capture 29](https://user-images.githubusercontent.com/92916632/146425235-884a873c-1f1c-43a7-bb4f-c99be6f84a9b.PNG)
  
 24. Restored the log files back into /var/log directory
  
          sudo rsync -av /home/recovery/logs/. /var/log
   ![Capture 30](https://user-images.githubusercontent.com/92916632/146426466-9176b450-cb11-411e-9b6d-26f62ca5e3d9.PNG)
   
 25.  Updated the /etc/fstab file so that mount configuration will persist after the restart of server
   
 - The UUID of the device will be used to update the /etc/fstab file 
    
 - Opened the blkid with the following command : 
    
        sudo blkid
     
![Capture 31](https://user-images.githubusercontent.com/92916632/146573591-c09c52b3-2090-43ef-8cb5-e3ab77cf3e98.PNG)
      
      
- Copied the UUID of the device. 

- Pasted on a notepad. 

- Removed the leading quotes & edited the ending quotes of the UUID 
   
![InkedCapture 31_LI](https://user-images.githubusercontent.com/92916632/146574056-91dae0ed-456b-41ce-ba70-7e80638e631a.jpg)


![Capture 35 editing UUID](https://user-images.githubusercontent.com/92916632/146582988-5663a25a-e321-488d-ba68-3558296e4dc1.PNG)
  
  
    
 - Opened the fstab file

        sudo vi /etc/fstab
    

 - Pasted the edited UUID in the fstab file
 
 - Saved and exited using :wq! enter 
 
 ![Capture 32](https://user-images.githubusercontent.com/92916632/146442533-4d1a4e12-4e59-45bd-b4a7-81897ec52828.PNG)


26. Tested the configuration and reloaded the daemon

         sudo mount -a
  
         sudo systemctl daemon-reload
  
  ![Capture 34](https://user-images.githubusercontent.com/92916632/146445651-06e8c0b7-8231-46b0-936b-3180cb9a290b.PNG)
  
  
 27.  Verified my setup by running : df -h
   ![Capture 33 ii df-h](https://user-images.githubusercontent.com/92916632/146446176-89cab909-bec7-4182-8f4a-24931832cf8e.PNG)
   
   
  
  Step 2- Prepared the Database Server
   
  
  1. Launched a second RedHat EC2 instance that will have a role – ‘DB Server’
 
 
  ![Capture 1](https://user-images.githubusercontent.com/92916632/146450850-e39d758c-f0b7-472e-bb53-8b9197fe6ce8.PNG)
   
  2. Created 3 volumes in the same availability zone as my database server EC2, each of 10G
   
  3.  Attached the 3 volumes one by one to my database server EC2 instance
   
  4.  To confirm that the newly created devices have been attached, i ran the command : lsblk
   
   The names of the the newly created block devices are xvdf, xvdg, xvdh as shown in the screenshot below
   ![Capture 2 lsblk](https://user-images.githubusercontent.com/92916632/146539898-7f16a412-96fc-4e29-9a82-c1b45fb3ea42.PNG)
   
  5. Used the follwing command to see all mounts and free space on my server :
   
         df -h
      
 6. Used gdisk utility to create a single partition on each of the 3 disks. For the creation of partition on the first disk, i ran the command below:
 
    
         sudo gdisk /dev/xvdf
      
 7.  Took the following steps :
    
  Typed n for new partition 
  
  Partition number : 1
  
  First sector (size which i want the disk used for) : clicked enter to use the entire disk
  
  Last sector : clicked enter to use entire disk
  
  Hex code or Guid = 8e00 to change partition type to Linux LVM 
  
  
  Command ( ? for help) : Typed p to check check the configuration
  
  Command ( ? for help) : Typed w to write 
  
  Do you want to proceed (Y/N) : Yes
  
  
  Screenshot showing the creation of partition on disk xvdf
  ![Capture 3 gdisk](https://user-images.githubusercontent.com/92916632/146545394-48c23429-c30a-452a-a4bd-1acb92ac18dc.PNG)
  
 
 8. For the creation of partition on the second disk, i ran the following command :
  
        sudo gdisk /dev/xvdg
  
   Followed the above named procedure which is also illustrated in the screenshot below
  ![Capture 4](https://user-images.githubusercontent.com/92916632/146545572-0a805947-2adf-4254-a236-e5afc2f91add.PNG)
  
 9.  For the creation of partition on the third disk, i ran the following command :
   
        sudo gdisk /dev/xvdh
  
  Followed the above named procedure which is also illustrated in the screenshot below
  ![Capture 5](https://user-images.githubusercontent.com/92916632/146546075-e067fdd7-c0e1-4166-8869-0733485edf38.PNG)
  
  
10.  To view the newly configured partition on each of the 3 disks. i ran : 
  
          lsblk
    
 ![Capture 6](https://user-images.githubusercontent.com/92916632/146560084-cf005d1c-bb7a-4a93-8ee1-5d3d42ec105c.PNG)
 
 
 11. Installed lvm2 package by running the command : 
 
         sudo yum install lvm2 -y
    
 12.  I marked each of the 3 disks as physical volume with the command :   
 
           sudo pvcreate /dev/xvdf1
     
           sudo pvcreate /dev/xvdg1
     
           sudo pvcreate /dev/xvdh1
      
 ![Capture 8](https://user-images.githubusercontent.com/92916632/146641276-4bd03f04-646c-42ca-9a88-754bab37a539.PNG)
      
 13.  Verified that my physical volume has been created by running the command :
  
            sudo pvs
       
 ![Capture 9](https://user-images.githubusercontent.com/92916632/146641318-a5478684-b7e8-4f45-b480-091419d56f1f.PNG)
       
 14.  Used vgcreate utility to add all 3 PVs to a volume group (VG).  I Named the Volume group vg-database
  
           sudo vgcreate vg-database /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
        
![Capture 10](https://user-images.githubusercontent.com/92916632/146641342-2a965fe0-399e-49dc-93d9-d37870fd118b.PNG) 
        
 15. Verified that my volume group has been created successfully by running the command : 
  
           sudo vgs
 
![Capture 11](https://user-images.githubusercontent.com/92916632/146641419-53c879b0-995a-46d0-b352-6a3a69312e9f.PNG)
         
 
         
  16. Used lvcreate utility to create 1 logical volume
   
           sudo lvcreate -n db-lv -L 20G vg-database
        
![Capture 12](https://user-images.githubusercontent.com/92916632/146641505-445d556e-cb3c-4128-938b-d17fd8c85e46.PNG)
   
   
  17.  Verified that my Lv has been created by running : 
   
           sudo lvs
        
 ![Capture 13](https://user-images.githubusercontent.com/92916632/146644046-b977733c-1c4c-4711-94fa-d7d92376cc1a.PNG)
        
 
 18. Created /db directory which is the database directory which also serves as the mounting point
  
            sudo mkdir /db
      
 19.  Used mkfs.ext4 to format the logical volume with ext4 filesystem
  
            sudo mkfs -t ext4 /dev/vg-database/db-lv
      
  ![Capture 15](https://user-images.githubusercontent.com/92916632/146642202-673d6081-1e21-49fd-8493-17706ba6587a.PNG)

      
 20.   Mounted /vg-database/db-lv on /db
  
           sudo mount /dev/vg-database/db-lv /db
      
  ![Capture 16](https://user-images.githubusercontent.com/92916632/146642216-b41db444-52d4-4d66-9190-5f09ace60409.PNG)
      
      
21.   Updated the /etc/fstab file so that mount configuration will persist after the restart of server
 
  -  The UUID of the device will be used to update the /etc/fstab file 
  
  -  Opened the blkid with the following command : 
  
           sudo blkid
       
  ![Capture 17](https://user-images.githubusercontent.com/92916632/146642273-6ff5e5e8-d2a3-4058-8a8a-4863b5ae06d3.PNG)

     
  -  Copied and pasted the UUID of the device on a notepad
  
  
  -  Removed the leading quotes and edited the ending quotes of the UUID
 
 ![Capture 21 notepad](https://user-images.githubusercontent.com/92916632/146643697-84fba381-3ec6-4764-85f9-c3bb06dc4b9a.PNG)
 
 

  -  Opened the fstab file 
 
          sudo vi /etc/fstab
      
 - Pasted the edited UUID in the fstab file
       
 - Saved and exited using : wq! enter
      

![Capture 18](https://user-images.githubusercontent.com/92916632/146615065-c7c829db-e94c-49fa-b908-fd674c2a52cb.PNG)
  
  
22.  Tested the configuration and reloaded the daemon
  
        sudo mount -a
      
        sudo systemctl daemon-reload
      
 ![Capture 19](https://user-images.githubusercontent.com/92916632/146643754-c1ec0c5b-2a44-43b9-8e2d-20ebfa944b39.PNG)

      
      
23.  Verified my setup by running : df -h
 
 ![Capture 20](https://user-images.githubusercontent.com/92916632/146643796-b9495d46-df71-4fa3-bb74-8584515bf46b.PNG)
 
 
     
 Step 3 - Installed wordpress on my web server EC2 
     
  1.  Updated the repository

           sudo yum -y update
       
  2.   Installed wget, Apache and its dependecies 
  
           sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
           
 ![Capture 2](https://user-images.githubusercontent.com/92916632/148656024-58934f4e-53de-4e1e-9111-affc441d7982.PNG)
           
  3.    Started Apache

            sudo systemctl enable httpd

            sudo systemctl start httpd
            
![Capture 3  started apache](https://user-images.githubusercontent.com/92916632/148656110-6fa278bf-811b-46dd-b791-70cc4398e244.PNG)
            
 4.   Installed PHP and its dependencies

            sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
            
            sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
            
            sudo yum module list php
            
            sudo yum module reset php
            
            sudo yum module enable php:remi-7.4
            
            sudo yum install php php-opcache php-gd php-curl php-mysqlnd
            
            sudo systemctl start php-fpm
            
            sudo systemctl enable php-fpm
            
             sudo setsebool -P httpd_execmem 1
             
 ![Capture 5  fedora](https://user-images.githubusercontent.com/92916632/148656411-0b2e1683-27a6-41e2-884a-82a92cc84867.PNG)

 ![Capture 6](https://user-images.githubusercontent.com/92916632/148656438-06d486ce-5337-47c4-881b-412d34de306c.PNG)
 
 ![Capture 7 module list](https://user-images.githubusercontent.com/92916632/148656503-4459b2aa-8a4f-44aa-95fb-8b02865e26b7.PNG)
 
 ![Capture 8 module reset](https://user-images.githubusercontent.com/92916632/148656534-e31112be-af8a-4fb6-876b-0b1455c9d245.PNG)
 
![Capture 10 php opache](https://user-images.githubusercontent.com/92916632/148656583-14e050e5-b628-420b-bb6f-f00af601a445.PNG)

![Capture 11 php version](https://user-images.githubusercontent.com/92916632/148656664-08c9f2dc-94e6-4b19-ad8b-6b55438e6a51.PNG)

![Capture 12](https://user-images.githubusercontent.com/92916632/148656681-38585b03-58be-4b5a-8943-d8b4857e7db1.PNG)

![Capture 13 status of php-fpm](https://user-images.githubusercontent.com/92916632/148657347-9635e8a4-e1ad-4b78-8335-5e3d8fc5fbb7.PNG)

![Capture 14 setbool](https://user-images.githubusercontent.com/92916632/148657068-1be24080-921e-48bd-bec1-f84e364c9236.PNG)

            
          
 5.   Restarted Apache

           sudo systemctl restart httpd
           
        
 6.   Downloaded wordpress and copied wordpress to var/www/html

      Created wordpress directory

             mkdir wordpress
           
      Changed directory into wordpress directory
            
             cd wordpress
             
      Downloaded wordpress
           
           sudo wget http://wordpress.org/latest.tar.gz
           
      ![Capture 15](https://user-images.githubusercontent.com/92916632/148658031-81b6d9ca-3b9a-43ab-898a-b959ea4a524f.PNG)
           
           
      Extracted the file 
           
           sudo tar xzvf latest.tar.gz 
           
    
![Capture 17 extracted the zip file](https://user-images.githubusercontent.com/92916632/148658090-b67525db-56bd-4903-ab21-ce453b06e87e.PNG)
           
          
  After extraction, an exta folder called wordpress was created
  
  ![Capture  extra folder](https://user-images.githubusercontent.com/92916632/148658730-e74b8e85-363c-47c8-b138-abc9a91be962.PNG)
 
 
  changed directory to wordpress, checked the contents of wordpress to confirm that it was extracted successfully 
    
           cd wordpress
          
           ls-l
          
   ![Capture cd wordpress and check the contents](https://user-images.githubusercontent.com/92916632/148659240-126a3aed-1259-46dc-81c2-014b4d629fdc.PNG)
          
         
  Copied the contents of wp-config-sample.php to wp-config.php 
         
            sudo cp -R wp-config-sample.php wp-config.php
           
            
  Copied the contents of wordpress into /var/www/html directory 
           
            sudo cp -R wordpress/. /var/www/html/
            
   ![Capture copy wordpress to var](https://user-images.githubusercontent.com/92916632/148661932-c23d7fdb-4378-4d4e-bf2f-0f0a3daf3dfc.PNG)
   
   ![Capture checked the contents of html](https://user-images.githubusercontent.com/92916632/148662047-d2bbbc96-b93b-4861-863a-99d28c55917b.PNG)
   
   
            
 7.   Configured SELinux Policies

            sudo chown -R apache:apache /var/www/html/
            
            sudo chcon -t httpd_sys_rw_content_t /var/www/html/ -R
           
            sudo setsebool -P httpd_can_network_connect=1
           
            sudo setsebool -P httpd_can_network_connect_db 1
            
           
 
   
  Step 4-   Installed mysql on webserver EC2
   
               sudo yum install mysql-server 
               
  Re-started the service and enabled it so it will run after reboot:
          
               sudo systemctl restart mysqld
               
               sudo systemctl enable mysqld
   
             
           
           
   Step 5-  Installed mysql on my DB server EC2
   
               sudo yum update
            
               sudo yum install mysql-server
               
   Verified that the service is up and running by using : 
   
               sudo systemctl status mysqld
   
   Service was not running, so i restarted the device and enabled it so it will be running after reboot: 
   
              sudo systemctl restart mysqld
              
              sudo systemctl enable mysqld
              
              sudo systemctl status mysqld
              
 ![Capture mysql status](https://user-images.githubusercontent.com/92916632/148533042-ebb58c00-b608-46c5-a012-8fce1e8a6a2d.PNG) 
 
 
 Step 6-  Configured Database to work with WordPress
 
           sudo mysql_secure installation 
           
 Logged in as mysql root user using password 
        
          sudo mysql -u root -p 
          
entered the password : password
          
          CREATE DATABASE wordpress;
          
          CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';
          
         GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
         
         FLUSH PRIVILEGES; 
         
         SHOW DATABASES;
         
      
  checked if my user has been created 
  
          select user, host from mysql.user;
          
   ![Capture create mysql database](https://user-images.githubusercontent.com/92916632/148698417-e21d2efc-2b1f-4b78-af94-9293e6873b68.PNG)
   ![Capture show mysql database](https://user-images.githubusercontent.com/92916632/148698471-64a3975a-5ae3-4d27-9d78-a588898060b0.PNG)
   
   
  
  Step 7-  Set the bind address for mysql database 
   
 Opened the config file and added the private I.P address of the webserver
  
                 sudo vi /etc/my.cnf
                 
  ![Capture etc config](https://user-images.githubusercontent.com/92916632/148703811-f3717883-c5d0-4ce6-bbf6-51d229334f25.PNG)
  ![Capture bind address 2](https://user-images.githubusercontent.com/92916632/148698835-31dbb0bc-3420-446a-8be8-3cbe4431cc0c.PNG) 
 
 
 Step 8-    Updated wp-config.php file on webserver 
 
   From the html directory, i ran the command below : 
   
                sudo vi wp-config.php
                
 ![Capture 20 open config php](https://user-images.githubusercontent.com/92916632/148697146-c3af68e5-3cf3-4bd7-83a0-4dd595b1a235.PNG)
 
 Edited the DB_name - 'wordpress' ,  DB_user - 'myuser' ,  DB_password - 'mypass' ,  DB_host - 'Private IP address of the database server' 
 
  saved and exited
  
  ![Capture  sc wp config php](https://user-images.githubusercontent.com/92916632/148696862-eac99103-a283-44d1-83c0-5b66a6f3e264.PNG)
  
   
  Re-started httpd
   
               sudo systemctl restart httpd
               
               
  Step 9-  Disabled the default page of Apache
   
               sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup
               
   
 Step 10-  Configured WordPress to connect to remote database.
 
 Opened MySQL port 3306 on DB Server EC2. This allows access to the DB server ONLY from my Web Server’s IP address, so in the 
 
  inbound Rule configuration, i specified source as : webserver private IP address /32
 
 ![Capture inbound rules](https://user-images.githubusercontent.com/92916632/148705033-2a144956-0c02-4ce6-b149-f322ee33ad76.PNG)
 
 
  Tested to see if i can connect from my Web Server to my DB server . From my webserver, i ran the command below :
 
           sudo mysql -h <DB-Server-Private-IP-address> -u myuser -p
           
 requested for password, and i entered the password : mypass
 
 successfully connected remotely as shown in the screenshot below
  
![Capture log into mysql](https://user-images.githubusercontent.com/92916632/148781451-f8026c1f-6d0d-4fe2-9ce1-63821c308b79.PNG)


Successfully executed SHOW DATABASES;

           show databases;
           
![Capture show databases](https://user-images.githubusercontent.com/92916632/148782260-641e358d-45dd-44ce-b67c-87a2f5274367.PNG)
          
          

Step 11-  From my browser, i accessed the link to my wordress

         http://<Web-Server-Public-IP-Address>
         
 ![Capture wordpress page 1](https://user-images.githubusercontent.com/92916632/148790770-01163805-ffa7-446c-a345-87c33ca8b4a7.PNG)  
 
![Capture wordpress page 2](https://user-images.githubusercontent.com/92916632/148791437-85b3b59e-f22a-40a5-afd7-99d4304eaa9f.PNG) 
              
              
  


        


     
     
  
     
  
  
  
  
  

  
 
  
  
 
  
  
  



  
  
   
   
  
  
  
  

  
  
  
  
 
 
 



 
 
