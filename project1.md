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
![14](https://user-images.githubusercontent.com/98546783/155352578-11e1be4c-6926-4fdc-b029-dc1b18d26b2b.jpg)
