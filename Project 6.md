
# WEB SOLUTION WITH WORDPRESS

# TASK - To prepare a storage infrastructure on two Linux servers and implement a basic web solution using WordPress
     
# To also ensure that the disks used to store files on the Linux servers were adequately partitioned and managed through  programs such as gdisk and LVM respectively.
 
 
 
 STEP 1 : Prepared a webserver
 
 Launched 2 EC2 instances that will serve as 'webserver' and 'database server' using Redhat OS
 
 ![Capture 1 ec2 instances](https://user-images.githubusercontent.com/92916632/146087488-3333bb1a-ed86-47e3-9294-988377be8bbf.PNG)

 
 Created 3 volumes in the same availability zone as my webserver EC2, each of 10G
 
 Attached the 3 volumes one by one to my webserver EC2 instance
 
 
