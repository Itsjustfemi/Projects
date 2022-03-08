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
populate the index.js file

Run node.js with
```
node index.js
```
Success is indicated by **Server running on port 5000** message
