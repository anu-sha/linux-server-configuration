# Linux Server Configuration
Steps to configure a ubuntu linux server and host a Flask application

#Steps

##Launching virtual machine

### References
* Udacity

1. Download private key and save it in the .ssh folder
2. Connect to the server as the root user using the key using Git Bash
⋅⋅* ssh -i ~/.ssh/udacity_key.rsa root@ip.address

##Create a new user grader

###References
* Udacity

1. Create the user
⋅⋅ *sudo adduser grader 

##Give sudo permissions to the user 'grader'

###References
* [http://askubuntu.com/questions/168280/how-do-i-grant-sudo-privileges-to-an-existing-user](http://askubuntu.com/questions/168280/how-do-i-grant-sudo-privileges-to-an-existing-user)

1. Use the command to edit the permissions
..* visudo 
2. Edit the file to add grader to the list of users with sudo permissions
..* grader ALL=(ALL:ALL) ALL 

##Login as grader and test sudo privileges

###References
* [https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)
* [http://stackoverflow.com/questions/22530886/ssh-copy-id-no-identities-found-error](http://stackoverflow.com/questions/22530886/ssh-copy-id-no-identities-found-error)

###Logging in as grader
1. Generate a public key using sshkeygen in a new command window
..* sshkeygen 
2. Save the key in a different file in .ssh folder
3. Connect to the server using the key and ipadress
..* ssh -i grader@ip-addreess 
4. If prompted for password, enter the grader password.

###Changing password configuration and checking sudo access
1. Edit ssh_config and sshd_config files to set PasswordAuthentication to no.This configuration forces users to login using keys.
..* sudo nano /etc/ssh/sshd_config 
..* sudo nano /etc/ssh/ssh_config 
2. Restart ssh
..* sudo service ssh restart 
3. Disconnect and login as the grader again. This time there will be no prompt for password.

###Checking sudo access
1. Login as grader and try running any command using sudo.
..* eg: visudo 
..* If the command works fine, the graer now has sudo privileges 

##Update currently installed packages

###References
..* Udacity 


