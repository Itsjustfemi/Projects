# PROJECT 2: LEMP STACK IMPLEMENTATION
Start my Ec2 Instance. Connect through SSH . e,g ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
 I had gitbash already installed.

Command 1$2 # We updated the ubuntu server and also installed the Nginx.

sudo apt update
sudo apt install nginx
**ROADBLOCK 1** The above two command ran well. However, when I ran the systemcheck command, it did not show green. Refer to screenshot below.
 
  ![Screenshot 2022-02-23 002428](https://user-images.githubusercontent.com/98546783/155239777-50b65aa1-9bdd-409c-8015-6ef84410d4e5.jpg)
  
  I tried installing the NGINX again and it showed me that it is already installed.
  
  I restared my EC2 and later got a different error as below
  
  ![2](https://user-images.githubusercontent.com/98546783/155240612-e33ad16f-3477-4e03-8a10-38aecdebbe20.jpg)

I tried restarting with the command "sudo systemctl restart nginx "and the issue was same.
  **RESOLUTION** 
 I created another EC2 instance after confirming online as per the error message that it was because of the APACHE running on my webserver that couldnot make it work
Then it started working after running the "sudo systemctl status nginx" cmd.

 
 ![image](https://user-images.githubusercontent.com/98546783/155240862-7006967f-7f1c-4a2b-b046-f2bbf7fcfa61.png)

 I opened my browser and typed the public Ip of my EC2 *http://<Public-IP-Address>:80* , I was able to see my nhinx webserver installed
 
 ![4](https://user-images.githubusercontent.com/98546783/155241511-9426c758-52f4-4046-84fc-2e4b2992f0b8.jpg)

 ## Installing webserver ##
 I used the apt cmd to install my web server
 sudo apt install mysql-server. WHen prompted, i selected 'y'
 
 ![5](https://user-images.githubusercontent.com/98546783/155241963-ce2df3d6-d4cc-4352-821a-8a01e4034318.jpg)
 I proceeded running some scripts to remove some insecure default settings and lock down access to your database system as below:
 sudo mysql_secure_installation -y
 
 ![7](https://user-images.githubusercontent.com/98546783/155242528-093bf81f-a970-412e-8bbd-74c12cfefe23.jpg)

Y was selected for every prompt. A password was prompted and selected for the MySQL root user.
 WHen through, I  was able to log in to the MySQL console by typing:

'sudo mysql'. Below is the result. I proceeded to log out using the 'exit' cmd.
 
 ![8](https://user-images.githubusercontent.com/98546783/155242942-2195ad7f-afda-4008-9b8a-09450e823acd.jpg)
 
 ## Instaling PHP ##
 I installed PHP to process code and generate dynamic content for the web server. ALl at once, I installed php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
 'sudo apt install php-fpm php-mysql'
 When prompted, I typed Y and press ENTER to confirm the installation. Reference screenshot below:
 
 ![9](https://user-images.githubusercontent.com/98546783/155243540-b6aac722-7c32-4b10-98a9-05ffd0663265.jpg)
 
**Configuring Nginx to Use PHP Processor**
 When using the Nginx web server, we can create server blocks to encapsulate configuration details and host more than one domain on a single server. I used *Projectdomain* as my sample domain.
 
I created a directory structure within /var/www for my your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites. 
 I created the root web directory for my domain using the command below:

sudo mkdir /var/www/projectdomain.
 
 The below command did the following with screenshot as reference below;
 
 1. Create the root web directory for my domain ================================ "sudo mkdir /var/www/projectdomain"
 
 2.Assign ownership of the directory with the $USER environment variable, which will reference my current system user: === "sudo chown -R $USER:$USER /var/www/projectdomain"
 
 3.open a new configuration file in Nginx’s sites-available directory using Nano == " sudo nano /etc/nginx/sites-available/projectdomain"
 
 
 
![11](https://user-images.githubusercontent.com/98546783/155245261-35cfa032-20f1-4988-83ff-d4b4bf40556c.jpg)

The command below was pasted in the config. file
 
 #/etc/nginx/sites-available/projectdomain

server {
    listen 80;
    server_name projectdomain www.projectdomain;
    root /var/www/projectdomain;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
 
 As per the block code above, below is what they do.
 *listen* — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.
*root* — Defines the document root where the files served by this website are stored.
 
*index* — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.
 
*server_name* — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.
 
 When i was done typing the command, I saved and closed the file using nano CTRL+X and then y and ENTER to confirm. After then I Activated my configuration by linking to the config file from Nginx’s sites-enabled directory: using the command below:
 
 sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

 Reference screenshot below:
 
 ![image](https://user-images.githubusercontent.com/98546783/155303745-a4d981f6-5b60-4dba-97c7-6781ce767211.png)
 

I ran the below code to test my cmd for syntax error and as shown, the result is sucessful.
 
 ![image](https://user-images.githubusercontent.com/98546783/155304291-b478a192-23eb-4022-aa67-e8897028fa46.png)
 
 The below 2cmds is to  disable default Nginx host that is currently configured to listen on port 80; and reload Nginx to apply the changes:
 
 'sudo unlink /etc/nginx/sites-enabled/default'
 'sudo systemctl reload nginx'
 
 ![image](https://user-images.githubusercontent.com/98546783/155305150-3eb25623-9ec1-4d93-b9e7-4ac586964bd8.png)
 
 
 Then I Created an index.html file in that location so that we can test that your new server block works as expected.
 
 sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectdomain/index.html
 
 
 ![image](https://user-images.githubusercontent.com/98546783/155305515-989a27ed-1079-4a71-92af-db23f22b72ff.png)
 
 
 So i proceeded to write check the browser on what I wrote on http://<Public-IP-Address>:80. I saw the exact same thing on the command above

 
 ![image](https://user-images.githubusercontent.com/98546783/155305817-7904b252-e294-41b4-9a6f-c055344a29f0.png)
 
 
 ### TESTING PHP WITH NGINX ###
 I Opened a new file called info.php within my document root in my text editor to confirm Nginx can correctly hand .php files off to my PHP processor:

 
sudo nano /var/www/projectdomain/info.php
 
 ![image](https://user-images.githubusercontent.com/98546783/155306632-65b6c7dc-7704-431e-a307-740c065abfb0.png)
 
 
 Afterwards, I pasted the php code and saved it. (to return information about my server)
 
 
 <?php
phpinfo();


i tried the cmd on my browsser and got the welcome message
http://`server_domain_or_IP`/info.php

![image](https://user-images.githubusercontent.com/98546783/155307893-555050a5-ad27-41eb-aea5-c6396bf123c0.png)


I used below cmd to remove the file I created:


sudo rm /var/www/projectdomain/info.php

![image](https://user-images.githubusercontent.com/98546783/155308312-9a77946f-4a74-4634-ba31-946f14c60f2a.png)


### RETRIEVING DATA FROM MYSQL DATABASE WITH PHP ###


The aim is to do a simple "To do list" and configure access to it, so the Nginx website 
would be able to query data from the DB and display it.

I ran the command below to connect to mysql
'sudo mysql'

To create a new database, I run the following command from your MySQL console:


 CREATE DATABASE `example_database`;        (the name of the database is example_database)


The below command was to create a new user **example_user** 
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

The below cmd was to give this user permission over the example_database database:

 GRANT ALL ON example_database.* TO 'example_user'@'%';

Refenence below screenshot:



![image](https://user-images.githubusercontent.com/98546783/155321042-f9087fdd-a6b2-4800-8927-a1fa942e417f.png)




![image](https://user-images.githubusercontent.com/98546783/155321381-db54712d-5960-49ae-9797-af558ec67e57.png)


![image](https://user-images.githubusercontent.com/98546783/155321654-409d7065-061a-4c36-96c5-4e842476af5e.png)

AFterwards, I exited.

mysql> exit

I tested if the newuser has permission in the database using:

mysql -u example_user -p


mysql> SHOW DATABASES;



![image](https://user-images.githubusercontent.com/98546783/155322953-64010115-1542-4e14-8ee7-2c37d94afe1d.png)




I proceeded to create a todo list:
![image](https://user-images.githubusercontent.com/98546783/155323435-1b242538-b59e-4e66-9e66-b392ef4c95e7.png)


i inserted the roles using *mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");*


 To confirm that the data was successfully saved to my table, I ran:


mysql>  SELECT * FROM example_database.todo_list;


Below is the the following output i got:
--------------------------------------------------------
![image](https://user-images.githubusercontent.com/98546783/155323893-eb507b2b-2731-4d88-b4c1-2039d8224d2f.png)
-------------------------------

I exited "exit"
Then i Created a new PHP file in my custom web root directory using vi


nano /var/www/projectdomain/todo_list.php


I pasted the code below
------------------------------

<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $p*****d);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

---------------------------
**ROADBLOCK2** :

After running the above code, and running http://<Public_domain_or_IP>/todo_list.php

I discovered an error message. I don't have the screenshot anymore but it was pointing to the password i used in the block of code above. 

I had to use the right password and accessed my public IP and it went through

![image](https://user-images.githubusercontent.com/98546783/155329462-e093c490-4761-4de0-bebb-fa8c8ad98903.png)



![12](https://user-images.githubusercontent.com/98546783/155330256-33d20e97-cf93-4a22-9321-3b9e1cde42bc.jpg)


The image above shows PHP environment is ready to connect and interact with your MySQL server.





