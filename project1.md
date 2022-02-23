# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS #
I already have my Ec2 instance created so I just connected  via ssh e,g ssh -i <Your-private-key.pem> ubuntu@
I already have gitbash and ubuntu terminal installed. 
**Roadblock1**
I was getting errors using my ubuntu terminal. Even after restaring the terminal. I had to installed gitbash and it seemed fine

![image](https://user-images.githubusercontent.com/98546783/155337283-6b54afdd-6be6-434d-8e2a-c98fac572156.png)
![image](https://user-images.githubusercontent.com/98546783/155337640-d411a701-0151-436d-be22-74991ce74680.png)

**INSTALLING APACHE AND UPDATING THE FIREWALL**
I updated my server and installed apache using the command below:
- sudo apt update
- sudo apt install apache2
I confirmed the status using 'sudo systemctl status apache2' It was showing green
![13](https://user-images.githubusercontent.com/98546783/155348264-bea02e40-b33c-4e92-9c6d-13c7dba42902.jpg)
![image](https://user-images.githubusercontent.com/98546783/155348563-fc825aa4-edbf-489e-a5f8-3dce8f17a8e3.png)

TCP port 80 (https)is already opened up in my inbound rule with IP 0.0.0.0/0 (which means all IP)
I tried accessing the Ubuntu shell by curl http://localhost:80 or curl http://127.0.0.1:80. It brought out list of commands
![image](https://user-images.githubusercontent.com/98546783/155349535-d230f321-e796-442e-bafd-15d7898547d6.png)
same as when i ran the below on my browser, it brought the apache page as shown below

![image](https://user-images.githubusercontent.com/98546783/155350305-cd10fe7f-f08c-4b3f-b06c-5cd7f6d08ffd.png)

- **INSTALLING MYSQL**

I ran the below cmd to install the mySQL database
sudo apt install mysql-server
Below is to run a security script that comes pre-installed with MySQL (press Y for everything)
sudo mysql_secure_installation

![image](https://user-images.githubusercontent.com/98546783/155351715-dd5add1b-53aa-4f17-971a-c02dd06fe9f8.png)
At some point i was asked to select a password MySQL root userand I did.
WHen finished i entered the mysql console using the command below and also got the screenshot below;
sudo mysql
![image](https://user-images.githubusercontent.com/98546783/155353312-d2cd6c8a-faea-4f3e-8e86-806ed1d3ea9b.png)
I exited using mysql> exit
**INSTALLING PHP**
The cmd below installes:
PHP which is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, I"ll install php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Lastly, I"ll need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
sudo apt install php libapache2-mod-php php-mysql

![image](https://user-images.githubusercontent.com/98546783/155356415-1d42b4c3-419c-4ec7-a44d-49eaab354148.png)

**CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE**
I will be using a domain called 'projectdomain.' Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.
I will be adding my own directory next to the default one with the cmd below:
sudo mkdir /var/www/projectdomain
Next cmd is to assign ownership of the directory
sudo chown -R $USER:$USER /var/www/projectdomain
 create and open a new configuration file in Apache’s sites-available directory using vi
 sudo vi /etc/apache2/sites-available/projectdomain.conf
 This will create a blank new file. We are inputing the below after pressing "i"
 <VirtualHost *:80>
    ServerName projectdomain
    ServerAlias www.projectdomain 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectdomain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
*Afterwwards, we Hit the esc button on the keyboard
Type :
Type wq. w for write and q for quit
Hit ENTER to save the file*
If we now use the ls cmd to view the file in the directory : 
sudo ls /etc/apache2/sites-available
![image](https://user-images.githubusercontent.com/98546783/155367797-c81aeb47-004d-4d87-889a-def58314a4ef.png)

I used a2ensite command to enable the new virtual host:
sudo a2ensite projectlamp
I disabled the default site that comes with APache using:
sudo a2dissite 000-default

To make sure your configuration file doesn’t contain syntax errors,  I ran:
sudo apache2ctl configtest and sudo systemctl reload apache2 to reload it

