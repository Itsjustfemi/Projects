## MERN STACK IMPLEMENTATION
On this project, I will create a SIMPLE TO-DO APPLICATION ON MERN WEB STACK, doing the following:
1. MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
2. ExpressJS: A server side Web Application framework for Node.js.
3. ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
4. Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

I started by initiating my EC2 instance and tagged it MERN STACK. This time i tried installing the windows terminal for the first time (already have gitbash and ubuntu)

![image](https://user-images.githubusercontent.com/98546783/157300456-5e50ff43-3147-4e94-9043-c5abf76e5fd1.png)

![image](https://user-images.githubusercontent.com/98546783/157300678-b187615e-8f93-421a-9672-9dcde0066b80.png)


I started by running the command to update and upgrade my server
```
sudo apt update -y && sudo apt upgrade -y
```
To get the location of Node.js software from Ubuntu repositories.

```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```
sudo apt-get install -y nodejs (instal nodejs on the server)
```
node -v && npm -v (node instalation and verification)

```
mkdir Todo
ls
cd Todo
```
npm init

npm init

. Install ExpressJS

```
npm install express
```
create index.js file

```
touch index.js
 Install dotenv
```
npm install dotenv

![image](https://user-images.githubusercontent.com/98546783/157543069-c1b5c4ba-bfc3-498e-8326-b040fe1afeaa.png)

```
populate the index.js file
Using vim, paste the code below in the file
```

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

```
Run node.js with
```
node index.js
```
Success is indicated by **Server running on port 5000** message

![image](https://user-images.githubusercontent.com/98546783/157541727-946283c6-acac-45a1-820d-dfda852ab583.png)

We then proceed to open port 5000 in our EC2 inbound rule (TCP)

![image](https://user-images.githubusercontent.com/98546783/157541221-d37ec3b0-46de-4796-99f5-f8aa5056e5f0.png)

I open my browser and paste `http://<PublicIP-or-PublicDNS>:5000`

![image](https://user-images.githubusercontent.com/98546783/157541814-12a16757-8cfc-454b-a7c4-8875a636bb00.png)

Our app should be able to
* Create a task (POST)
* Display list of tasks (GET)
* Remove completed tasks (DELETE)

in the todo directory, create routes directory and move into it

mkdir routes && cd routes

create api.js file and populate it

```
touch api.js
```

- To make our app interactive, we need *Models*

Go into todo directory, Install moongose, a no-sql database and create todo.js file
```
mkdir models && cd models && touch todo.js

```
