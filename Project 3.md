## MERN STACK IMPLEMENTATION


   STEP 1 : Connected to the server through mobaxterm SSH client by taking the following steps: 
        
   Created a new session by clicking session
        
   Clicked the SSH tab & pasted my public IP address into the remote host box
         
   Specified username as Ubuntu
        
   Clicked on advanced SSH settings,clicked on use private key,and uploaded my PEM key and then clicked ok
        
        

 STEP 2 : BACKEND CONFIGURATION 

 Updated ubuntu OS by running the command : sudo apt update
          
 Upgraded Ubuntu by running the command :  sudo apt upgrade
          
 I got the location of Node.js software from Ubuntu repositories by running : curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
 
 ![Capture 4 got the location of Node js software](https://user-images.githubusercontent.com/92916632/141571222-e7af3bcf-561d-4235-a643-42e9e1d572bf.PNG)
 
 Installed Node.js on the server with the command: sudo apt-get install -y nodejs
 
 The command above installs both nodejs and npm. npm is a package manager for node 
 
 Confirmed the node installation by running the command : node -v
 
 Confirmed the npm installation by running the command: npm -v
 
 ![Capture 5 Installed   confirmed Node js on the server](https://user-images.githubusercontent.com/92916632/141573327-91a873d8-6656-4375-a52a-d48df3c4f398.PNG)
 
 
 



APPLICATION CODE SET UP

Created a new directory for my To-Do project: mkdir Todo 

Verified that the Todo directory is created with ls command : ls

Changed my current directory to the newly created one : cd Todo

Ran the command :  npm init to initialise my project so that a new file named package.json will be created. Pressed 

enter several times to accept default values, edited the description( A To-Do Application),key words(todo application),

author(Opeyemi), then accepted to write out the package.json file by typing yes

Ran the ls command to confirm that package.json file has been created


![Capture 6 created a new directory](https://user-images.githubusercontent.com/92916632/141576868-e78f68f2-f87b-4b3c-86b8-c09f34e654af.PNG)


![Capture 8](https://user-images.githubusercontent.com/92916632/141594996-efc0a1c8-6d8d-4494-80aa-055762a4e9ee.PNG)



INSTALLED EXPRESSJS

Intalled expressjs using : npm install express

Created the index.js file using : touch index.js

Ran ls to confirm that my index.js file is successfully created

Installed the dotenv module using : npm install dotenv

![Capture 9 install expressjs   created index  js file](https://user-images.githubusercontent.com/92916632/141645463-dcf5b7b6-bb31-4434-aa21-ad25f2077410.PNG)

Opened the index.js file with the command: vim index.js

Pasted the following code into the index.js file :

const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

used :w to save and :qa to exit vim

To confirm the content of the index.js file, i ran cat index.js

  ![Capture 10 to confirm the content of index js file](https://user-images.githubusercontent.com/92916632/141645560-bb198b64-8176-4d99-bd29-9cf7501e4055.PNG)





Tested to see if the server works by typing : node index.js


![Capture 11 tested to see if server running](https://user-images.githubusercontent.com/92916632/141645590-a8b96e5f-3372-41f8-81a0-290b869def18.PNG)




 Added a new inbound rule to open TCP port 5000

![Capture 12 opened port 5000](https://user-images.githubusercontent.com/92916632/141645235-d07a87e5-90fc-4b74-b77c-2f16f27c5f68.PNG)



Opened my web page and pasted my public ip address and :5000

![Capture 13 express webpage](https://user-images.githubusercontent.com/92916632/141645387-66979a43-0e77-44c4-a7ba-e562142eb0d9.PNG)


Opened another shell on mobaxterm, connected to my ec2 

changed directory to Todo : cd Todo

Typed ls

Created a folder called routes using : mkdir routes

Changed directory to routes folder using :cd routes

Created a blank file api.js with the command : touch api.js

Opened the file with the command: vim api.js

Copied the following code into the file : 

const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

Typed : wq and enter to save and exit

Ran the command cat api.js to check if the code has been saved in the file 

![Capture 15  created a new route and file](https://user-images.githubusercontent.com/92916632/141644894-37ba5701-61f5-4a28-94e7-988f92f3d082.PNG)





 MODELS
 
 To create a schema and a model, installed mongoose which is a Node.js package that makes working with Mongodb easier.
 
 To install Monoose, i changed back to Todo folder with cd .. 
  
 Intalled Mongoose with : npm install mongoose
 
 Created a new folder called models with : mkdir models
 
 Changed directory into the newly created ‘models’ folder with : cd models
 
 Inside the models folder, created a file and named it todo.js : touch todo.js
 
 Alternatively, all the 3 command above can be defined in one line & concurently with the help of && operator, like this:
 
 mkdir models && cd models && touch todo.js
 
 Opened the file created(todo.js) with : vim todo.js ,copied & pasted the following code inside :
 
 const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;

I need to to update my routes from the file api.js in 'routes' directory' to make use of the new model. Took the ff steps:

Ran the command : cd .. to go back to Todo directory, changed directory to routes by running the command : cd routes

Typed ls to confirm the api.js file

In routes directory, opened the api.js file with : vim api.js , deleted the code inside with : %d and enter

Copied and pasted the code below into it, saved and exited using :wq and enter

const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
 
To confirm that the code was saved, i ran the command: cat api.js

![Capture 18a  to be merged](https://user-images.githubusercontent.com/92916632/141745919-0fda7344-3a02-4802-8c51-91570f35a00e.PNG)
![Capture 18b](https://user-images.githubusercontent.com/92916632/141746035-146d8abf-44bd-4422-91eb-f8a7443229f1.PNG)


  


        
        
