# SHELL SCRIPTING
## **The script purpose will do the following;**
- This script will read a CSV file that contains 20 new Linux users.
- This script will create each user on the server and add to an existing group called 'Developers'.
- This script will first check for the existence of the user on the system, before it will attempt to create that it.
- The user that is being created also must also have a default home folder
- Each user should have a .ssh folder within its HOME folder. If it does not exist, then it will be created.
- For each user’s SSH configuration, We will create an authorized_keys file and add the below public key.
 
 * Below is the script that would be used for this project

 ---

```
#!/bin/bash
userfile=$(cat names.csv)
PASSWORD=password

# To ensure the user running this script has sudo privilege
    if [ $(id -u) -eq 0 ]; then

# Reading the CSV file
	for user in $userfile;
	do
            echo $user
        if id "$user" &>/dev/null
        then
            echo "User Exist"
        else

# This will create a new user
        useradd -m -d /home/$user -s /bin/bash -g developers $user
        echo "New User Created"
        echo


# This will create a ssh folder in the user home folder
        su - -c "mkdir ~/.ssh" $user
        echo ".ssh directory created for new user"
        echo

# We need to set the user permission for the ssh dir
         su - -c "chmod 700 ~/.ssh" $user
         echo "user permission for .ssh directory set"
         echo

# This will create an authorized-key file
        su - -c "touch ~/.ssh/authorized_keys" $user
        echo "Authorized Key File Created"
        echo

# We need to set permission for the key file
        su - -c "chmod 600 ~/.ssh/authorized_keys" $user
        echo "user permission for the Authorized Key File set"
        echo

# We need to create and set public key for users in the server
        cp -R "/root/onboard/id_rsa.pub" "/home/$user/.ssh/authorized_keys"
        echo "Copyied the Public Key to New User Account on the server"
        echo
        echo

        echo "USER CREATED"

# Generate a password.
sudo echo -e "$PASSWORD\n$PASSWORD" | sudo passwd "$user" 
sudo passwd -x 5 $user
            fi
        done
    else
    echo "Only Admin Can Onboard A User"
    fi

```

- I started by creating my Ec2 instance and left it running. Then I copied my shell script from local server to the 
remote server (Ec2 instance) using `scp` command. Screeshot below:

* Now i ssh to my instance and when i did an 'ls', i sAW the onboard.sh file there. Screenshot below
* 

![ls onborad sh](https://user-images.githubusercontent.com/98546783/172025798-93ee7de9-f995-467b-86c5-7843b616f5d6.jpg)


* 
![scp confirm](https://user-images.githubusercontent.com/98546783/172025506-cdc004da-6cc8-4fc6-8b38-94f5ae559a51.jpg)


* Now we create a folder called shell and move into it (screenshot)


![create shell and move into it](https://user-images.githubusercontent.com/98546783/172025558-a5990d77-cba2-48fb-b580-95a00c04b5d9.jpg)

Now we move the onboard.sh from the ubuntu dir to the shell dir.

` mv onboard.sh ~/shell `

Now we changed directory to the shell directory and create the public key , private key and the csv files for names

` touch id_rsa id_rsa.pub names.csv`

- we open the public key and put the key inside

`vi id_rsa.pub`
 
- I did the same for the csv file and private key file as well. In the csv file, i put in some names there that would have permission to access the server. (screenshot below)


![cat names csv](https://user-images.githubusercontent.com/98546783/172025619-dcceac39-c3db-453e-a4bb-8596d5cf008c.jpg)


- I opened the onboard script (`vi onboard.sh`) and changed the path to point to my public folder; so it can copy to the authorised keys of the remote folders. (screenshot below)

![changed path](https://user-images.githubusercontent.com/98546783/172025654-ca5bf5c3-222a-42a4-85fc-aa1fc9e8eb81.jpg)



- After that, I proceeded in creating the group developers using `sudo groupadd developers`

- Now its time to run my scripting, but before then, i needed to give the .sh file executable permission

`sudo chmod +x onboard.sh`

![executable permission for sh](https://user-images.githubusercontent.com/98546783/172025857-49ab00cf-3b65-45c2-b4f2-0f68508de693.jpg)


- i changed to a root user using `sudo su` and ran the `.onboard.sh` cmd (screenshot below)
- 
![onboard sh gangan](https://user-images.githubusercontent.com/98546783/172025833-2610a125-17c2-45d3-85d2-074f1abfe5fa.jpg)

- To further verify; (below screenshot)
- 
![check names](https://user-images.githubusercontent.com/98546783/172025875-5d8d2082-26d6-4b1b-ad02-b37ef8501c52.jpg)

* Now we try testing one of the created user to ssh to the server. Before we do that, we will create a keypair (private )on the physical machine the user can log in with. I basically just created the private key on my machine that will connect to the instance (pub key)
* 
![vi auxproject pem](https://user-images.githubusercontent.com/98546783/172025927-6e26b2ff-f2d2-4ef0-ac9b-1b7234d48c94.jpg)

![femi connecting to server](https://user-images.githubusercontent.com/98546783/172025952-8d7bf615-32db-4023-95e0-347656362912.jpg)

- After conecting as femi, I did an `ls` and could see that the group developers is there


- I did an `ls` whle connected as femi and also an ls inside the .ssh folder (screenshot below). The files are intact as per the script. I could see the pub key in the authorized file.
- 
![ls femi ls  ssh](https://user-images.githubusercontent.com/98546783/172025988-9ecd943f-29ec-47d3-942e-2335c7ad920c.jpg)
