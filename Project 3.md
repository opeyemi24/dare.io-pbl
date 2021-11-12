## MERN STACK IMPLEMENTATION


   STEP 1 : Connected to the server through mobaxterm SSH client by taking the following steps: 
        
   Created a new session by clicking session
        
   Clicked the SSH tab & pasted my public IP address into the remote host box
         
   Specified username as Ubuntu
        
   Clicked on advanced SSH settings,clicked on use private key,and uploaded my PEM key and then clicked ok
        
        

 STEP 2 : Backend configuration 

 Updated ubuntu OS by running the command : sudo apt update
          
 Upgraded Ubuntu by running the command :  sudo apt upgrade
          
 I got the location of Node.js software from Ubuntu repositories by running : curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
 
 ![Capture 4 got the location of Node js software](https://user-images.githubusercontent.com/92916632/141571222-e7af3bcf-561d-4235-a643-42e9e1d572bf.PNG)
 
 Installed Node.js on the server with the command: sudo apt-get install -y nodejs
 
 Confirmed the node installation by running the command : node -v
 
 Confirmed the npm installation by running the command: npm -v
 
 ![Capture 5 Installed   confirmed Node js on the server](https://user-images.githubusercontent.com/92916632/141573327-91a873d8-6656-4375-a52a-d48df3c4f398.PNG)
 
 
 



Application Code Setup

Created a new directory for my To-Do project: mkdir Todo 

Verified that the Todo directory is created with ls command : ls

Changed my current directory to the newly created one : cd Todo

Ran the command :  npm init to initialise my project so that a new file named package.json will be created. Pressed 

enter several times to accept default values, edited the description( A to do app), key words(todo application),

author(Opeyemi), then accepted to write out the package.json file by typing yes

Ran the ls command to confirm that package.json file has been created


![Capture 6 created a new directory](https://user-images.githubusercontent.com/92916632/141576868-e78f68f2-f87b-4b3c-86b8-c09f34e654af.PNG)


![Capture 8](https://user-images.githubusercontent.com/92916632/141594996-efc0a1c8-6d8d-4494-80aa-055762a4e9ee.PNG)


        
        
