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
```
![image](https://user-images.githubusercontent.com/98546783/157543069-c1b5c4ba-bfc3-498e-8326-b040fe1afeaa.png)

![s](https://user-images.githubusercontent.com/98546783/157543736-1d85c80b-ee2c-424d-814b-4fd9db733645.jpg)

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
 MONGODB Database
mLab provide Mongodb as a DBaaS, click [here](https://www.mongodb.com/atlas-signup-from-mlab) to sign up
Create an account and then setup your project with it database
Copy the connection string as seen below

moongose connection string as below:


![image](https://user-images.githubusercontent.com/98546783/157560576-6f62e27b-473b-474d-aa0e-6b9096a16997.png)


Ofcourse we already enabled the Ip address to anywhere
![image](https://user-images.githubusercontent.com/98546783/157560441-8c294da5-412c-4753-a526-0386c4aca7dd.png)


Copy and paste the content of the connection string into .env file you will create in todo directory Update the content of index.js file to reflect that .env file is being used for database connection purposes

Run the engine again with
node index.js

POSTMAN Is a browser on steroids. It will be used to test the API We will perform CRUD (Creat, Read, Update and Delete)
See the screenshots below to see how it is done with Postman

![image](https://user-images.githubusercontent.com/98546783/157551287-91d7f5dd-d3dd-4a32-b554-eb92f6a77a25.png)
![image](https://user-images.githubusercontent.com/98546783/157552667-b2ead802-0496-4012-8318-295e91875233.png)
![image](https://user-images.githubusercontent.com/98546783/157552748-7334a997-e92c-4742-bd9f-5b0c68ae6e82.png)
![image](https://user-images.githubusercontent.com/98546783/157552806-c81137c0-5f15-4daf-bda3-c8d6436f6394.png)
![image](https://user-images.githubusercontent.com/98546783/157554098-155f0586-6d52-433f-8a9e-ae753259addf.png)

FRONTEND CREATION
In the todo directory, 

`run npx create-react-app client`
this will create a new folder in your Todo directory called client
Run the following dependency below:

`npm install concurrently --save-dev`

Install nodemon. It is used to run and monitor the server. 

`npm install nodemon --save-dev`

paste below in the package.json file inside todo folder
``` 

"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},

```

`cd client`

- Open the package.json file


`vi package.json`
Add the key value pair in the package.json file "proxy": "http://localhost:5000".

Now, ensure you are inside the Todo directory, and simply do:

`npm run dev`

***Creating your React Components***

`cd client`

move to the src directory

`cd src`
Inside your src folder create another folder called components

mkdir components

`cd components`
Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

`touch Input.js ListTodo.js Todo.js`
`vi Input.js`

```

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
```

To make use of Axios 
Move to the src folder

cd ..
Move to clients folder

cd ..
Install Axios

npm install axios

cd src/components

vi ListTodo.js
paste 

```

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
```

Then in your Todo.js file you write the following code

```

import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
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
```

We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.

Move to the src folder

cd ..
Make sure that you are in the src folder and run

vi App.js
Copy and paste the code below into it
```
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
```

In the src directory open the App.css

vi App.css
Then paste the following code into App.css:
```

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

```

In the src directory open the index.css

vim index.css
Copy and paste the code below:

```
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

```

Go to the Todo directory

cd ../..
When you are in the Todo directory run:

npm run dev
![image](https://user-images.githubusercontent.com/98546783/157556929-06f4ea67-6eaa-4c4e-a3db-b98d35c78d0f.png)

Populate the files with appropriate contents

Open port 3000 for incoming TCP traffic Set brower to your Public IP and port 3000 ip-address:3000
![image](https://user-images.githubusercontent.com/98546783/157557432-c57810d7-e7cc-40bd-b15d-8852f48081af.png)

## ROAD BLOCK ##
The app was not showing on my public browser at first. Only the logo was showing 
![image](https://user-images.githubusercontent.com/98546783/157557773-59bc84ad-c963-4eeb-9200-8a40cd37e00a.png)

It was until I doublechecked on my ListTodo.js folder, I saw using the `command` that it was empty. So i inputed the right script accordinly
