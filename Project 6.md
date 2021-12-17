
# WEB SOLUTION WITH WORDPRESS

# TASK - To prepare a storage infrastructure on two Linux servers and implement a basic web solution using WordPress
     
# To also ensure that the disks used to store files on the Linux servers were adequately partitioned and managed through  programs such as gdisk and LVM respectively.
 
 
 
 STEP 1 : Prepared a webserver
 
 Launched an EC2 instances that will serve as 'webserver' using Redhat OS
 
 Screenshot showing the creation of EC2 instance
 ![Capture 1b](https://user-images.githubusercontent.com/92916632/146448622-190d94aa-e332-403f-857d-66ae084f22b3.PNG)


 
 Created 3 volumes in the same availability zone as my webserver EC2, each of 10G
 
 Screenshot showing the creation of volumes 
 
 ![Capture 2 creation of volumes](https://user-images.githubusercontent.com/92916632/146151256-7640a96f-eda4-49be-a084-f7f941a2a67c.PNG)

 
 
 Attached the 3 volumes one by one to my webserver EC2 instance
 
  Screenshot showing the attachment of volumes to webserver EC2 instance
 
 ![Capture 3 attach volume 1](https://user-images.githubusercontent.com/92916632/146151490-cc439dab-300d-45a5-b013-11b7858f7c7e.PNG)
 
 ![Capture 3c attach vol 2](https://user-images.githubusercontent.com/92916632/146151846-56c51d87-8ea3-4642-93aa-7f5d828604d5.PNG)
 
 ![Capture 3d attach vol 3](https://user-images.githubusercontent.com/92916632/146151917-c0a7e6ca-95be-473c-802f-648fe0cbe818.PNG) 
 
 
 To confirm that the newly created devices have been attached, i ran the command : lsblk
 
 The names of the the newly created block devices are xvdf, xvdg, xvdh as shown in the screenshot below 
 
 ![Capture 7 lsblk command](https://user-images.githubusercontent.com/92916632/146231101-6d74c41f-099a-4345-bf8b-96d96e6b687f.PNG)

 
  All devices in Linux reside in /dev/ directory. To inspect we can run ls /dev/
  
  ![Capture 8](https://user-images.githubusercontent.com/92916632/146231535-4350eb20-ce8f-464c-8392-ddee2c1306fd.PNG)
  
  
  To see all mounts and free space on my server, i ran df -h 
  
   Screenshot showing that the 3 newly added devices have not been mounted
  ![Capture 9](https://user-images.githubusercontent.com/92916632/146231757-f7ebeba3-f6b0-4c71-8926-d2c141cd42ab.PNG)
  
  
  I used gdisk utility to create a single partition on each of the 3 disks. To create partition on the first disk, i ran the command below:
  
  
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
  
  
  For the creation of partition on the second disk, i ran the follwoing command:
  
     sudo gdisk /dev/xvdg 
  
  Followed the above named procedure which is also illustrated in the screenshot below
   ![Capture 11](https://user-images.githubusercontent.com/92916632/146241325-7b03585a-994b-47df-8a76-e805a1aa685d.PNG)
   
   For the creation of partition on the third disk, i ran the command :
   
     sudo gdisk /dev/xdgf
  
  Followed the above named procedure which is also illustrated in the screenshot below
   ![Capture 12](https://user-images.githubusercontent.com/92916632/146241861-9cc04cd9-a78a-4f17-aa69-23f76f5ebc47.PNG)
  
  To view the newly configured partition on each of the 3 disks. i ran : lsblk
   ![Capture 13 to confirm configuration](https://user-images.githubusercontent.com/92916632/146242086-275cf978-2993-4028-b985-365d7c06a9fa.PNG)
   
 
  Physical volumes will be created on these partitons.
  
  Installed lvm2 package using : sudo yum install lvm2 -y
  ![Capture 14 install lvm2 package](https://user-images.githubusercontent.com/92916632/146272224-357405da-8669-4147-b1c1-1c28c6c95502.PNG)
  
  
  I marked each of the 3 disks as physical volume with the command :   
  
        sudo pvcreate /dev/xvdf1
 
        sudo pvcreate /dev/xvdg1
  
        sudo pvcreate /dev/xvdh1
  
 ![Capture 18](https://user-images.githubusercontent.com/92916632/146274626-b07ea3da-3ca2-4dce-b6ee-f87391a8de71.PNG)
  
  Verified that my physical volume has been created by running : sudo pvs
  ![Capture 15 confirm physical volume](https://user-images.githubusercontent.com/92916632/146273030-c5181b09-9c28-4bc9-8d5b-a2bb60d2d0b3.PNG)
  
 
  Used vgcreate utility to add all 3 PVs to a volume group (VG).  I Named the VG webdata-vg 
  
          sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
  
  ![Capture 16 creation of volume group](https://user-images.githubusercontent.com/92916632/146273540-04c40c55-0df9-4645-986e-67a6e6dc5733.PNG)
  
  Verified that my VG has been created successfully by running : sudo vgs
  ![Capture 17 verified that volume group has been created](https://user-images.githubusercontent.com/92916632/146274109-f8a261e5-d5e9-40bf-b824-0befa7dcbaf5.PNG)
  
  Used lvcreate utility to create 2 logical volumes.  app-lv(i used half of the physical volume size) and log-lv( i used the remaining space of the pv size). 
  These logical volumes are the actual disks to be allocated to the servers.
  
  app-lv will be used to store data for the website, while log-lv will be used to store data for logs
  
          sudo lvcreate -n apps-lv -L 14G webdata-vg
  
          sudo lvcreate -n logs-lv -L 14G webdata-vg
  
  ![Capture 18 create logical volume](https://user-images.githubusercontent.com/92916632/146404757-c50429d9-725f-49d4-a079-9e93066936cc.PNG)
  
  Verified that my logical volume has been created by running : sudo lvs
  ![Capture 19 verify creation of logical volume](https://user-images.githubusercontent.com/92916632/146405492-fd81f5ff-15f3-4ef0-bd94-14544dcc593f.PNG)
  
  
  Verified the entire set up 
  
         sudo lsblk
  
  ![Capture 20 verify the entire set up](https://user-images.githubusercontent.com/92916632/146406561-dfcdc2ae-880d-4dd8-a864-6dd7efa1325f.PNG)
  
  I used mkfs.ext4 to format the logical volumes with ext4 filesystem
  
       sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
  
       sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
  
  ![Capture 23 combination of 2 file system](https://user-images.githubusercontent.com/92916632/146411537-d881ae6c-53dc-4ebd-9c0e-4405c020c4f7.PNG)


   Created  /var/www/html directory to store website files
    
       sudo mkdir -p /var/www/html
   ![Capture 24](https://user-images.githubusercontent.com/92916632/146422196-61d5021e-e51f-42fe-9f8f-95d5940d9062.PNG)
   
   Created /home/recovery/logs to store backup of log data
   
       sudo mkdir -p /home/recovery/logs
   ![Capture 25](https://user-images.githubusercontent.com/92916632/146422430-ece1e3cb-f301-47cb-9fe1-9aae598a9235.PNG)
   
   Mounted /var/www/html on apps-lv logical volume
   
       sudo mount /dev/webdata-vg/apps-lv /var/www/html/
   ![Capture 27](https://user-images.githubusercontent.com/92916632/146423741-2f0e5fd4-fdee-427e-92bd-568028ebbaec.PNG)
   
   Used rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)
   
       sudo rsync -av /var/log/. /home/recovery/logs/
   
   ![Capture 28](https://user-images.githubusercontent.com/92916632/146424483-5aed6f32-ca7b-472e-8609-b1ad42bc0c1c.PNG) 
   
   Mounted /var/log on logs-lv logical volume. (All the existing data on /var/log will be deleted. That is why step 18 above is very important)
   
      sudo mount /dev/webdata-vg/logs-lv /var/log
   
  ![Capture 29](https://user-images.githubusercontent.com/92916632/146425235-884a873c-1f1c-43a7-bb4f-c99be6f84a9b.PNG)
  
  Restored the log files back into /var/log directory
  
     sudo rsync -av /home/recovery/logs/. /var/log
   ![Capture 30](https://user-images.githubusercontent.com/92916632/146426466-9176b450-cb11-411e-9b6d-26f62ca5e3d9.PNG)
   
  
Updated /etc/fstab file so that the mount configuration will persist after restart of the server.
   
    sudo vi /etc/fstab
    
Updated /etc/fstab in the format below using my own UUID. I also removed the leading quotes and edited the ending quotes as shown in the screenshot below.
    
![Capture 32](https://user-images.githubusercontent.com/92916632/146442533-4d1a4e12-4e59-45bd-b4a7-81897ec52828.PNG)

Tested the configuration and reloaded the daemon

      sudo mount -a
  
      sudo systemctl daemon-reload
  
  ![Capture 34](https://user-images.githubusercontent.com/92916632/146445651-06e8c0b7-8231-46b0-936b-3180cb9a290b.PNG)
  
  
   Verified my setup by running : df -h
   ![Capture 33 ii df-h](https://user-images.githubusercontent.com/92916632/146446176-89cab909-bec7-4182-8f4a-24931832cf8e.PNG)
   
   
  
  Step 2- Prepared the Database Server
   
  
  Launched a second RedHat EC2 instance that will have a role – ‘DB Server’
 
 
  ![Capture 1](https://user-images.githubusercontent.com/92916632/146450850-e39d758c-f0b7-472e-bb53-8b9197fe6ce8.PNG)
   
   Created 3 volumes in the same availability zone as my webserver EC2, each of 10G
   
   Attached the 3 volumes one by one to my webserver EC2 instance
   
   To confirm that the newly created devices have been attached, i ran the command : lsblk
   
   The names of the the newly created block devices are xvdf, xvdg, xvdh as shown in the screenshot below
   ![Capture 2 lsblk](https://user-images.githubusercontent.com/92916632/146539898-7f16a412-96fc-4e29-9a82-c1b45fb3ea42.PNG)
   
   Used the follwing command to see all mounts and free space on my server :
   
      df -h
      
  Used gdisk utility to create a single partition on each of the 3 disks. For creation of partition on the first disk, i ran the command below:
 
    
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
  
  
  Screenshot showing the creation of partition on disk xvdf
  ![Capture 3 gdisk](https://user-images.githubusercontent.com/92916632/146545394-48c23429-c30a-452a-a4bd-1acb92ac18dc.PNG)
  
 
 For the creation of partition on the second disk, i ran the following command :
  
      sudo gdisk /dev/xvdg
  
   Followed the above named procedure which is also illustrated in the screenshot below
  ![Capture 4](https://user-images.githubusercontent.com/92916632/146545572-0a805947-2adf-4254-a236-e5afc2f91add.PNG)
  
   For the creation of partition on the third disk, i ran the following command :
   
      sudo gdisk /dev/xvdh
  
  Followed the above named procedure which is also illustrated in the screenshot below
  ![Capture 5](https://user-images.githubusercontent.com/92916632/146546075-e067fdd7-c0e1-4166-8869-0733485edf38.PNG)
  

  
 
  
  
 
  
  
  



  
  
   
   
  
  
  
  

  
  
  
  
 
 
 



 
 
