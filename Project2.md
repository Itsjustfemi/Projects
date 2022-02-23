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
 
 The below command did the following with screenshot as refercne below;
 1. Create the root web directory for my domain ================================ "sudo mkdir /var/www/projectdomain"
 2.Assign ownership of the directory with the $USER environment variable, which will reference my current system user: === "sudo chown -R $USER:$USER /var/www/projectdomain"
 3.open a new configuration file in Nginx’s sites-available directory using Nano == " sudo nano /etc/nginx/sites-available/projectdomain"
 
 
 
![11](https://user-images.githubusercontent.com/98546783/155245261-35cfa032-20f1-4988-83ff-d4b4bf40556c.jpg)

