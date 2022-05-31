### Building up a MEAN STACK (Mongobd, Express,Angular & Node.js)

The Mean Stack is a combination of the following components:

- MongoDB (Document Database): Stores and allows to retrieval of data.
- Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
- Angular (Front-end application framework) – Handles Client and Server Requests.
- Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user
* Launch your EC2 Instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM) and connect.

We start by installing Node.js is a JavaScript runtime. Before then we start by updating/upgrading our server and adding certificates
`sudo apt update`

`sudo apt upgrade`

`sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates`

`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

- Now we install nodejs
`sudo apt install -y nodejs`

### Installing MongoDB 

Below command to import the MongoDB public GPG Key.
`wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -`

The operation should respond with an OK.

- I proceeded with creating a list key value

`echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list`

- Now we install the MongoDB packages.

`sudo apt-get install -y mongodb-org`

`- sudo service mongod start`
- I ran the below cmd to start the server and verify its working
- 
`sudo systemctl status mongod`

![Screenshot 2022-05-31 162801](https://user-images.githubusercontent.com/98546783/171211378-c163f9f5-711c-4d59-ad2a-2430247baa03.jpg)

The next command was to **Install npm** – Node package manager and **body-parser**’ package to help us process JSON files passed in request

`sudo apt install -y npm`
`sudo npm install body-parser`
Now we Create a folder named ‘Books’, Initialize npm project & add a file to it named server.js

`mkdir Books && cd Books `

`npm init`
`mkdir Books && cd Books`

I now Copied and pasted the web server code below into the server.js file as shown in screenshot below (vi server.js).

![vi serverjs](https://user-images.githubusercontent.com/98546783/171212319-f59a7f92-5349-4548-840f-2ab5da2132ab.jpg)

### INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER 
We start by installing Express and set up routes to the server
(Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications)

`sudo npm install express mongoose`
`mkdir apps && cd apps`

Now we create a file named routes.js

In the ‘apps’ folder, I created a folder named models, entered (cd) into the directory and created a file named book.js. Finally, i put some code in it. (screenshot)

![routesjs](https://user-images.githubusercontent.com/98546783/171213014-dd4ae815-a68b-46c3-92b4-8dd49a0c3c6d.jpg)

- In the ‘apps’ folder, create a folder named models, open the book and put the block of code below
`mkdir models && cd models`
`vi book.js`
```
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
```
### Access the routes with AngularJS
I Changed the directory back to ‘Books’,Created a folder named public and added a file named script.js

`cd ../.. `

`mkdir public && cd public`

` vi script.js`

![scriptjs](https://user-images.githubusercontent.com/98546783/171216920-a78a2564-856c-4e9e-a6b5-49f829ef5c59.jpg)


* In public folder, create a file named index.html; and putthe below code inside.
`vi index.html`

```
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
```

Now we change the directory back up to Books
`cd ..`

we Started the server by running this command:

`node server.js`

Below was the result

![nodeserverjs](https://user-images.githubusercontent.com/98546783/171206766-33c556a5-e8fc-4a90-9675-c67786bc909c.jpg)

- To connect and see the content on a the internet, we need to open TCP port 3300 on AWS Web Console for our EC2 Instance
- 
- ![port3300](https://user-images.githubusercontent.com/98546783/171218454-7890fca4-fb22-4675-951c-9a9f8b718c00.jpg)
- 
- Now we can access our Book Register web application from the internet with a browser using Public IP address or Public DNS name.

This is the content of the Web Book Register Application using a browser with information entered. Check the screenshot below.

![web book app](https://user-images.githubusercontent.com/98546783/171219200-f4b6e7a7-af01-4992-9e26-97353db7302c.jpg)

### ROAD BLOCKS
- I was having issues with running all this on ubuntu server 22.04. I guess because of some compatible dependecies. I changed to Ubuntu 20.04
- While installing mongodb, i had to follow their website instruction squarely as I encountered issues again
  https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/ 
- I also observed when trying checking the mongodb status, while using of `sudo systemctl status mongodb`,i was havng issues. Rather, I was using `sudo systemctl status mongod` (without b)
- ![Screenshot 2022-05-31 162801](https://user-images.githubusercontent.com/98546783/171224291-cfea619f-a2dd-4097-b306-e9c6501fe772.jpg)
