## MERN STACK IMPLEMENTATION

## In this project,i created a simple To-Do application and deployed it to MERN STACK. 

## I wrote a front end application using React.js that communicates with a backend application written using Expressjs. 

## I also created a Mongodb backend for storing tasks in a  database

   


   Connected to the server through mobaxterm SSH client by taking the following steps: 
        
   Created a new session by clicking session
        
   Clicked the SSH tab & pasted my public IP address into the remote host box
         
   Specified username as Ubuntu
        
   Clicked on advanced SSH settings,clicked on use private key,and uploaded my PEM key and then clicked ok
        
        

 STEP 1 : BACKEND CONFIGURATION 

 Updated ubuntu OS by running the command : sudo apt update
          
 Upgraded Ubuntu by running the command :  sudo apt upgrade
          
 I got the location of Node.js software from Ubuntu repositories by running : curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
 
 ![Capture 4 got the location of Node js software](https://user-images.githubusercontent.com/92916632/141571222-e7af3bcf-561d-4235-a643-42e9e1d572bf.PNG)
 
 Installed Node.js on the server with the command: sudo apt-get install -y nodejs
 
 The command above installs both nodejs and npm. npm is a package manager for node 
 
 Confirmed the node installation by running the command : node -v
 
 Confirmed the npm installation by running the command: npm -v
 
 ![Capture 5 Installed   confirmed Node js on the server](https://user-images.githubusercontent.com/92916632/141573327-91a873d8-6656-4375-a52a-d48df3c4f398.PNG)
 
 
 



.APPLICATION CODE SET UP

Created a new directory for my To-Do project: mkdir Todo 

Verified that the Todo directory is created with ls command : ls

Changed my current directory to the newly created one : cd Todo

Ran the command :  npm init to initialise my project so that a new file named package.json will be created. Pressed 

enter several times to accept default values, edited the description( A To-Do Application),key words(todo application),

author(Opeyemi), then accepted to write out the package.json file by typing yes

Ran the ls command to confirm that package.json file has been created


![Capture 6 created a new directory](https://user-images.githubusercontent.com/92916632/141576868-e78f68f2-f87b-4b3c-86b8-c09f34e654af.PNG)


![Capture 8](https://user-images.githubusercontent.com/92916632/141594996-efc0a1c8-6d8d-4494-80aa-055762a4e9ee.PNG)



.INSTALLED EXPRESSJS

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





 .MODELS
 
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


.MONGODB DATABASE

signed up on MLAB and created a mongodb database and collection

Created a file in my Todo directory & named it .env with : touch .env

Opened up the file with : vi .env


Added the connection string to access the database in it just as below :

    DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
    
    Updated <username>, <password>, <network-address> and <database> according to  my setup on Mlab

 Ran the the command cat .env to check if the connection string was added into the env file
 
 Updated the index.js file to reflect the use of .env so that node can connect to the database. I did that by deleting 
 
 the existing content in the file and replacing with the the code below. To delete the existing content i took the following steps:
 
 1. Open the file with vim index.js 
 2. Press esc 
 3. Type :
 4. Type %d
 5. Hit ‘Enter’

The entire content was deleted, then,

 6. Press i to enter the insert mode in vim
 7. Now, pasted the entire code below in the file.

const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

Saved and exited with :wq and enter

Ran the command : cat index.js to confirm that code was copied

![Capture 21 env file with cat](https://user-images.githubusercontent.com/92916632/142028940-1a9e29d3-2ab8-4472-b93c-48c2d5d9f37d.PNG)

Started my server using the command: node index.js

![Capture 22 Database connected successfully](https://user-images.githubusercontent.com/92916632/142030723-03fbd22c-5ad7-478e-b9e6-dc0daf128354.PNG)


    
 .Used postman to test API
 
 Created a POST reqest to the API
 
 ![Capture Post request](https://user-images.githubusercontent.com/92916632/142032298-c59c9436-e5d8-4093-94d7-a67da6ca7cdd.PNG)
 

 
 Created a  GET request to the API

![Capture get request](https://user-images.githubusercontent.com/92916632/142031876-d4fe92bc-3b79-449f-ae5a-4d667470a4da.PNG)



 STEP 2 : FRONT END CREATION
 
 In the same root directory as my backend code which is Todo directory, i ran:  npx create-react-app client
 
 This created a new folder called client which is where i added all the react code.
 
   Running a react app
   
  1. Installed concurrently, a dependency which is used to run more than one command simultenously from the same terminal window
   
   npm install concurrently --save-dev
   
   2. Installed Nodemon, a dependency which is used to run and monitor the server
   
   npm install nodemon --save-dev
   
   
  ![Capture 23 install concurently   nodemon](https://user-images.githubusercontent.com/92916632/142781204-dd5cecfc-bdc5-4cbf-8392-f5d835a980e8.PNG)

   
  3.  In the Todo folder, i opened the package.json file using : vi package.json
   
   I deleted the line containing : 
   
                                   scripts     
                                   test 
                                   }
   
   Replaced with the following code :   
   
                                   "scripts": {
                                   "start": "node index.js",
                                   "start-watch": "nodemon index.js",
                                   "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
                                   },
   
   ![Capture 24 replacing code](https://user-images.githubusercontent.com/92916632/142781450-2008ccf6-2e08-4b57-8fbf-1d94956d8a19.PNG)

  
   
   Configure proxy in package.json 
   
   1. Changed directory to 'client' : cd client

   2. Opened the package.json file : vi package.json

   3. Added the key value pair in the package.json file using: "proxy": "http://localhost:5000"

  
   ![Capture 25 added key pair value](https://user-images.githubusercontent.com/92916632/142781580-c5b21c3c-9c9d-4b26-ad1b-951ff93169f0.PNG)

  
  Saved and exited.
      
  Cd back to todo directory & ran : npm run dev
  
  ![Capture 26 ran npm run dev](https://user-images.githubusercontent.com/92916632/142781715-7e4a46ca-fa61-4701-8eb1-cc94f65ee22e.PNG)
      
  Opened TCP port 3000 on EC2  by adding a new security group
      
  Copied and pasted my public Ip address, added :3000
  
  ![Capture 27 react app](https://user-images.githubusercontent.com/92916632/142781805-48d26108-da31-457f-ad67-a53cca38afd6.PNG)
  
  
  
  Creating react components
  
   From Todo directory, i changed to client directory : cd client
   
   Moved to src directory : cd src
   
   Inside src folder, created another folder called components : mkdir components
   
   Typed ls to confirm that the folder 'components' was created
   
   Changed directory into the components folder : cd components
   
   Inside the components directory, created 3 files Input.js, ListTodo.js and Todo.js.  : touch Input.js ListTodo.js Todo.js
   
   Typed ls to confirm that the 3 files were successfully created
   
   Opened the Input.js file : vi Input.js
   
   Copied and pasted the following code: 
   
import React, { Component } from 'react';

import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input

Typed :wq! and enter to save and exit vim editor

Ran : cat Input.js to confirm that code was copied and successfully saved.


![Capture 28b creating a react component](https://user-images.githubusercontent.com/92916632/142782535-c02bd934-6584-42f7-b446-199866044772.PNG)


To make use of Axios, i took the following steps : 

Moved to the src folder  :  cd ..

Moved to the clients folder  : cd .. 

From the client's folder i installed Axios : npm install axios

![Capture 29 installed Axios](https://user-images.githubusercontent.com/92916632/142782995-693971c2-efab-435b-8687-b93845fbd193.PNG)


I changed directory into components directory : cd src/components

After that i opened ListTodojs : vi ListTodo.js

In the ListTodo.js copied and pasted the following code :    

    import React from 'react';

    const ListTodo = ({ todos, deleteTodo }) => {

    return (
    <ul>
    {
    todos &&
    todos.length > 0 ?
    (
    todos.map(todo => {
    return (
    <li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
    )
    })
    )
    :
    (
    <li>No todo(s) left</li>
    )
    }
    </ul>
    )
    }

    export default ListTodo
    
 Saved and exit by typing :wq! and enter
 
 
 Opened the todo.js : vi todo.js
 
 copied & pasted the following code : 
 
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = 
{
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;


Saved and exited by typing : wq and enter


I need to make little adjustment to the react code. Delete the logo and adjust the App.js

 Moved to the src folder : cd ..
 
 From the src folder ran the command : vi App.js
 
 deleted the contents by typing : %d & enter
 
 Copied & pasted the following code : 
 
     import React from 'react';

     import Todo from './components/Todo';
     import './App.css';

     const App = () => {
     return (
     <div className="App">
     <Todo />
     </div>
     );
     }

     export default App;
     
 saved and exited vim editor by typing : wq! and enter
     
     
 In the src directory, opened the App.css : vi App.css
     
 deleted the contents with : %d and enter
     
 pasted the following code: 
     
    .App {
    text-align: center;
    font-size: calc(10px + 2vmin);
    width: 60%;
    margin-left: auto;
    margin-right: auto;
    }

    input {
    height: 40px;
    width: 50%;
    border: none;
    border-bottom: 2px #101113 solid;
    background: none;
    font-size: 1.5rem;
    color: #787a80;
    }

    input:focus {
    outline: none;
    }

    button {
    width: 25%;
    height: 45px;
    border: none;
    margin-left: 10px;
    font-size: 25px;
    background: #101113;
    border-radius: 5px;
    color: #787a80;
    cursor: pointer;
    }

    button:focus {
    outline: none;
    }

    ul {
    list-style: none;
    text-align: left;
    padding: 15px;
    background: #171a1f;
    border-radius: 5px;
    }

    li {
    padding: 15px;
    font-size: 1.5rem;
    margin-bottom: 15px;
    background: #282c34;
    border-radius: 5px;
    overflow-wrap: break-word;
    cursor: pointer;
    }

    @media only screen and (min-width: 300px) {
    .App {
    width: 80%;
    }

    input {
    width: 100%
    }

    button {
    width: 100%;
    margin-top: 15px;
    margin-left: 0;
    }
    }

    @media only screen and (min-width: 640px) {
    .App {
    width: 60%;
    }

    input {
    width: 50%;
    }

    button {
    width: 30%;
    margin-left: 10px;
    margin-top: 0;
    }
    }
    
  Saved and exited using : wq! and enter
    
  In the src directory, opened the index.css : vim index.css
    
  deleted the contents with : %d and enter
    
  copied and pasted the code below : 
    
    body {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    box-sizing: border-box;
    background-color: #282c34;
    color: #787a80;
    }

    code {
    font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
    }
    
 saved and exited with : wq! and enter
    
 Went back to todo directory :  cd ../..
    
 from Todo directory, i ran : npm run dev
    
    
![Capture 31](https://user-images.githubusercontent.com/92916632/142865667-e9d19d17-52c0-4267-99de-0fcb4618979c.PNG)


To test the functionality of my To-Do app, i opened my webpage, copied and pasted my public IP address :3000

![Capture 32 tested my todo app](https://user-images.githubusercontent.com/92916632/142866748-0145c5eb-ec03-4673-9a42-8c3423b476cf.PNG)
    
    


           

   
   
   
   
   







  


        
        
