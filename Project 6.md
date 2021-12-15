
# WEB SOLUTION WITH WORDPRESS

# TASK - To prepare a storage infrastructure on two Linux servers and implement a basic web solution using WordPress
     
# To also ensure that the disks used to store files on the Linux servers were adequately partitioned and managed through  programs such as gdisk and LVM respectively.
 
 
 
 STEP 1 : Prepared a webserver
 
 Launched 2 EC2 instances that will serve as 'webserver' and 'database server' using Redhat OS
 
 Screenshot showing the creation of EC2 instances
 
 ![Capture 1 ec2 instances](https://user-images.githubusercontent.com/92916632/146087488-3333bb1a-ed86-47e3-9294-988377be8bbf.PNG)

 
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
  
  
  I used gdisk utility to create a single partition on each of the 3 disks
  
  I followed the following steps :
  
  Ran the command : sudo gdisk /dev/xvdf
  
  Typed n for new partition 
  
  Partition number : 1
  
  First sector (size which i want the disk used for) : clicked enter to use the entire disk
  
  Last sector : clicked enter to use entire disk
  
  Hex code or Guid = 8e00 to change partition type to Linux LVM 
  
  
  Command ( ? for help) : Typed p to check check the configuration
  
  Command ( ? for help) : Typed w to write 
  
  Do you want to proceed (Y/N) : Yes
  
  
   Screenshot showing the creation of partition of disk xvdf
  ![Capture capture 10](https://user-images.githubusercontent.com/92916632/146240112-d016c1ab-0f0d-4ceb-9de4-5a0cb79d0812.PNG)
  
  Followed the above named procedure for the creation of partition of the second disk
   ![Capture 11](https://user-images.githubusercontent.com/92916632/146241325-7b03585a-994b-47df-8a76-e805a1aa685d.PNG)
  
  Followed the above named procedure for the creation of partion of the third disk
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
  
  Used lvcreate utility to create 2 logical volumes app-lv(i used half of the physical volume size) and log-lv( i used the remaining space of the pv size)
  
  app-lv will be used to store data for the website, while log-lv will be used to store data for logs
  
  sudo lvcreate -n apps-lv -L 14G webdata-vg
  
  sudo lvcreate -n logs-lv -L 14G webdata-vg
  
  Verified that my logical volume has been created by running : sudo lvs
  
  
  Verified the entire set up 
  
  
  
  



  
  
   
   
  
  
  
  

  
  
  
  
 
 
 



 
 
