
## MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

## In this project, i implemented a simple Book Register web form using MEAN stack.

MEAN Stack is a combination of following components:

1. MongoDB (Document database) – Stores and allows to retrieve data
2. Express (Back-end application framework) – Makes requests to Database for Reads and Writes
3. Angular (Front-end application framework) – Handles Client and Server Requests
4. Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user

Connected to the server through mobaxterm SSH client by taking the following steps:

Created a new session by clicking session

Clicked the SSH tab & pasted my public IP address into the remote host box

Specified username as Ubuntu

Clicked on advanced SSH settings,clicked on use private key,and uploaded my PEM key and then clicked ok


STEP 1 : INSTALLED NODEJS

Updated Ubuntu with the command  :  sudo apt update

![Capture 1 updated ubuntu](https://user-images.githubusercontent.com/92916632/144457814-8f8ba78e-bfa6-4212-bde5-28b29e634fec.PNG)

Upgraded Ubuntu with the command :  sudo apt upgrade

![Capture 2 upgraded ubuntu](https://user-images.githubusercontent.com/92916632/144458284-189237c2-b1b4-40be-9db0-95078bcce57f.PNG)


Added certificates : 

sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

 Installed Nodejs with the command : sudo apt install -y nodejs
 
 Confirmed the version of Nodejs installed by running : node  --version
 
 ![Capture 3 installed Nodejs](https://user-images.githubusercontent.com/92916632/144459108-d3adfdfe-e7df-49ee-b406-090727a9d411.PNG)
 
 
 Step 2: INSTALLED MONGODB
 
 Added MongoDB key to the key server by pasting the following command : 
 
 sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
 
 
 Added a repository for MongoDB with the following command : 
  
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list


![Capture 4 Added Mongodb to kerserver   added a repository for Mogodb](https://user-images.githubusercontent.com/92916632/144470512-29f1a322-fcd5-46db-a4f9-c421e3d224b5.PNG)


Installed MongoDB with the following command :

sudo apt install -y mongodb

![Capture 5 installed Mongodb](https://user-images.githubusercontent.com/92916632/144470954-13a41933-cac1-4589-aff8-038165f09686.PNG)

Started the server : sudo service mongodb start

Verified that server is up and running : sudo systemctl status mongodb

![Capture 6 started server and verified that service is up and running](https://user-images.githubusercontent.com/92916632/144471269-2f0c5888-7b91-4eac-a145-ef2ec746e910.PNG)


Installed npm-Node package manager with the command below :

                                     sudo apt install aptitude
                                     
                                     sudo aptitude install -y npm
                                     
![Capture 7 installed npm](https://user-images.githubusercontent.com/92916632/144471833-8e4dbe68-a2f9-42ea-b887-5fdb5af4a02f.PNG)


Installed body-parser package. This helps to process Json files passed in requests to the server.

sudo npm install body-parser

![Capture 8 Installed body parser](https://user-images.githubusercontent.com/92916632/144472658-78970d4c-85ab-4d84-b2ab-9bd12151d139.PNG)


Created a route directory, a folder where the application will be built. I named the directory 'Books' :

mkdir Books && cd Books

In the Books directory, i initialised npm project : npm init

Description : a simple book register, entry point : server.js, license : MIT

![Capture 9 mkdir and cd books, npm init](https://user-images.githubusercontent.com/92916632/144492515-18c7eef8-fa2d-4e4d-b9df-335114a3a2a3.PNG)



Added a file to it named server.js. To create the new file i used : vi server.js

Typed i for insert mode in vim

copied and pasted the web server code below into the server.js file 

 var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});


 Typed esc : wq! enter to save and exit vim editor
 
 
 STEP 3 : INSTALLED EXPRESS AND SET UP ROUTES TO THE SERVER
 
 Installed express and mongoose using : sudo npm install express mongoose
 
 
![Capture 10 installed express mongoose](https://user-images.githubusercontent.com/92916632/144517387-6c73e999-664c-4a29-9120-1491e1a27572.PNG)

In 'Books' folder, i created a folder named apps : mkdir apps && cd apps 

Created a file named routes.js : touch routes.js

Opened the routes.js file      : vi  routes.js

Copied and pasted the following code into the routes.js file :

var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};

Typed esc, : wq! enter to save and exit

In the 'apps' folder created a folder named models : mkdir models && cd models

In the models folder, created a file named book.js : touch book.js

Opened the book.js file using : vi book.js

Typed i (insert)

Copied and pasted the following code : 

var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);


Typed esc, : wq! enter to save and exit vim editor


![Capture 11](https://user-images.githubusercontent.com/92916632/144601061-2509ed08-a12d-4070-96ca-80d671c7dbe3.PNG)



STEP 4 : Access the routes with AngularJS

Changed the directory back to 'Books' : cd ../..

Created a folder named 'public' and cd into the public folder : mkdir public && cd public

From the 'public folder', created a file named script.js : vi script.js

Typed i (insert)

Copied and pasted the code below into the script.js file 



var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});


Typed esc, :wq! enter to save and exit vim editor

In the public folder, created a file named index.html : vi index.html  

The command above created and opened the index.html file on vim editor

Typed i (insert)

Copied and pasted the front end code below into the index.html file



<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>


Typed esc, :wq enter to save and exit

Ran the command cat index.html to confirm that code was copied and saved successfully



![Capture 12](https://user-images.githubusercontent.com/92916632/144676065-b0851ca8-8d93-4da2-bed5-c167291b1404.PNG)



Changed the directory back to 'books' : cd ..

Started the server by running this command : node server.js

![Capture 13  started the server   server up](https://user-images.githubusercontent.com/92916632/144678708-0bc29fc9-77cc-49a7-8622-375f74fff1e7.PNG)


Launched a seperate SSH console to test what curl command returns locally : curl -s http://localhost:3300

It returned the html page below: 

![Capture 14 Tested loccaly using curl command](https://user-images.githubusercontent.com/92916632/144679794-1b52edff-9628-4697-b16b-d3ed3634bd7a.PNG)


Opened TCP port 3300 in my AWS web console 


![Capture 15 opened port 3300](https://user-images.githubusercontent.com/92916632/144680035-4325af11-7dd7-42d3-8edc-e2085141ac56.PNG)


Accessed my Book register web application from the internet by pasting my public IP address :3300 into a browser

![Capture 16 Accessed my book register web application](https://user-images.githubusercontent.com/92916632/144680736-f45e232c-ae9b-4f01-a1da-9a8475a65de3.PNG)










                                                   




     































 
 











 
 


 


