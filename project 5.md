### IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS)

- On this project, I am creating and configuring two Linux-based virtual servers so that they can communicate to each other using local IP addresses (as opposed to SSH)

- We start by creating 2 EC2 instances calling it 

Server A name - `mysql server`
Server B name - `mysql client` 

- The two servers are already on the same default VPC and would be on same subnet
* On mysql server Linux Server, we  install MySQL Server software.
- ` sudo apt update -y `

- `sudo apt install mysql-server -y`

- `sudo systemctl enable mysql`

- We also replicate the same for the client server

- ` sudo apt update -y `

- `sudo apt install mysql-client -y`
- Now we edit the inbound rule for the clent server to accept traffic from only mysql client. Before we can do that we need to figure out the private ip address of the mysql-client using using `ip addr show` with port 3306(mysql/aurora)
- ![image](https://user-images.githubusercontent.com/98546783/173861446-7a5bdd3a-6fe4-42d2-9dab-9c90461beb70.png)

![image](https://user-images.githubusercontent.com/98546783/173861757-e3ed7d25-0d69-43bd-88e0-04620403600e.png)



- Now we run the security installation script for securing the mysql server deployment,

`sudo mysql_secure_installation`

- It prompted some security questions which i answered as deem fit. In my own case, NO, YES, YES, YES, YES.

- Now we log in to the mysql environmnent using 

`sudo mysql`

- Now we create a user named remote user

` CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; `

- Then we proceed to create a database.

` CREATE DATABASE test_db`

- NExt off, I proceeded to grant privileges;

` GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH OPTION;`

Next we flush privileges;
- ` FLUSH PRIVILEGES; `

`exit`
![image](https://user-images.githubusercontent.com/98546783/173862251-95a883b3-8c80-48de-ae08-59bdf99c1e68.png)


- Now apart frpm the inbound rule we specified, I configured the mysql server to allow connection from the remote host. I configured the mysqldcnf file and replaced '127.0.0.1' with '0.0.0.0' on the bind address
- 
![image](https://user-images.githubusercontent.com/98546783/173862699-0cbe9ea8-2d08-4d05-9b95-79c1d6488216.png)


- Now we restart the service

`sudo systemctl restart mysql`

### *Now the real deal, we try connecting from our Linux mysql-server to my-sql-client without SSH*

- Now on the mysql-client, we run;

`sudo mysql -u remote_user -h 172.31.44.251 -p`

(the ipaddrsss in the command is the priavte ip address of the mysql-server  )

(The '-p' means password prompt')

- Once we authenticate sucessfully, then we are in the database. When we run `show database'; we see the same thing just as when we run it on our mysql server.

![image](https://user-images.githubusercontent.com/98546783/173862981-45b96b96-4027-4064-a909-055a9ca00d68.png)


## ROAD BLOCK
-------

- when i tried `sudo my-sql` to my server, i kept on getting "Error 1045 (28000); Access denied fo user 'root'@localhost' (using password: NO)

- I kept on having it even after using ` mysql -u root`

- It worked after i tried 

- `mysql -u root -p`
