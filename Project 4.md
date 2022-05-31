### Building up a MEAN STACK (Mongobd, Express,Angular & Node.js)

We start by installing Node.js is a JavaScript runtime. Before then we start by updating/upgrading our server and adding certificates
`sudo apt update`

`sudo apt upgrade`

`sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates`

`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

Now we install nodejs
`sudo apt install -y nodejs`

### Installing MongoDB 

Below command to import the MongoDB public GPG Key.
`wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -`

The operation should respond with an OK.

I proceeded with creating a list key value

`echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list`

Now we install the MongoDB packages.

`sudo apt-get install -y mongodb-org`

I ran the below cmd to start the server and verify its working

`sudo service mongod start`
`sudo systemctl status mongod`

The next command was to **Install npm** – Node package manager and **body-parser**’ package to help us process JSON files passed in request

`sudo apt install -y npm`
`sudo npm install body-parser`
Now we Create a folder named ‘Books’, Initialize npm project & add a file to it named server.js

`mkdir Books && cd Books `

`npm init`
`mkdir Books && cd Books`

I now Copied and pasted the web server code below into the server.js file as shown in screenshot below (vi server.js).

### INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER 
We start by installing Express and set up routes to the server
(Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications)

`sudo npm install express mongoose`
`mkdir apps && cd apps`

Now we create a file named routes.js

In the ‘apps’ folder, I created a folder named models, entered (cd) into the directory and created a file named book.js. Finally, i put some code in it. (screenshot)

`mkdir models && cd models`
`vi book.js`

### Access the routes with AngularJS
I Changed the directory back to ‘Books’,Created a folder named public and added a file named script.js

`cd ../.. `
`mkdir public && cd public`
` vi script.js`

In public folder, create a file named index.html; and put code inside.
`vi index.html`

Now we change the directory back up to Books & Started the server by running this command:

`cd ..`
`node server.js`
