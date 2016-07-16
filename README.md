# Linux Server Configuration
Steps to configure a ubuntu linux server and host a Flask application

#Steps

##Launching virtual machine

### References
* Udacity

1. Download private key and save it in the .ssh folder
2. Connect to the server as the root user using the key using Git Bash
..* ssh -i ~/.ssh/udacity_key.rsa root@ip.address

##Create a new user grader

###References
* Udacity

1. Create the user
..* sudo adduser grader *..

##Give sudo permissions to the user 'grader'

###References
* [http://askubuntu.com/questions/168280/how-do-i-grant-sudo-privileges-to-an-existing-user](http://askubuntu.com/questions/168280/how-do-i-grant-sudo-privileges-to-an-existing-user)

1. Use the command to edit the permissions
..* visudo *..
2. Edit the file to add grader to the list of users with sudo permissions
..* grader ALL=(ALL:ALL) ALL *..
