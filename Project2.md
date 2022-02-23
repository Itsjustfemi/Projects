# PROJECT 2: LEMP STACK IMPLEMENTATION
Start my Ec2 Instance. Connect through SSH . e,g ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>

Command 1$2 # We updated the ubuntu server and also installed the Nginx.

sudo apt update
sudo apt install nginx
**ROADBLOCK 1** The above two command ran well. However, when I ran the systemcheck command, it did not show green. Refer to screenshot below.
 
  ![Screenshot 2022-02-23 002428](https://user-images.githubusercontent.com/98546783/155239777-50b65aa1-9bdd-409c-8015-6ef84410d4e5.jpg)
  
  I tried installing the NGINX again and it showed me that it is already installed.
  
  I restared my EC2 and later got a different error as below
  
  ![2](https://user-images.githubusercontent.com/98546783/155240612-e33ad16f-3477-4e03-8a10-38aecdebbe20.jpg)

I tried restarting with the command "sudo systemctl restart nginx "and the issue was same.
  **RESOLUTION** I created another EC2 instance after confirming online as per the error message that it was because of the APACHE running on my webserver that couldnot make it work
