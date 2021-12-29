
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









