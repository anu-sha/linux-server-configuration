# Linux Server Configuration
Steps to configure a ubuntu linux server and host a Flask application

##App Details:
1. IP address - 52.26.98.76
2. SSH Port - 2200
3. Url - [http://ec2-52-26-98-76.us-west-2.compute.amazonaws.com/](http://ec2-52-26-98-76.us-west-2.compute.amazonaws.com/)


#Steps

##Launching virtual machine

### References
* Udacity

1. Download private key and save it in the .ssh folder
2. Connect to the server as the root user using the key using Git Bash
  * ssh -i ~/.ssh/udacity_key.rsa root@ip.address

##Create a new user grader

###References
* Udacity

1. Create the user
⋅⋅ *sudo adduser grader 

##Give sudo permissions to the user 'grader'

###References
* [http://askubuntu.com/questions/168280/how-do-i-grant-sudo-privileges-to-an-existing-user](http://askubuntu.com/questions/168280/how-do-i-grant-sudo-privileges-to-an-existing-user)

1. Use the command to edit the permissions
  * visudo 
2. Edit the file to add grader to the list of users with sudo permissions
  * grader ALL=(ALL:ALL) ALL 

##Login as grader and test sudo privileges

###References
* [https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)
* [http://stackoverflow.com/questions/22530886/ssh-copy-id-no-identities-found-error](http://stackoverflow.com/questions/22530886/ssh-copy-id-no-identities-found-error)
* [http://askubuntu.com/questions/244115/how-do-i-enforce-a-password-complexity-policy](http://askubuntu.com/questions/244115/how-do-i-enforce-a-password-complexity-policy)

###Logging in as grader
1. Generate a public key using sshkeygen in a new command window
  * sshkeygen 
2. Save the key in a different file in .ssh folder
3. Connect to the server using the key and ipadress
  * ssh -i pathtokey grader@ip-address 
4. If prompted for password, enter the grader password.

###Changing password configuration and checking sudo access
1. Edit ssh_config and sshd_config files to set PasswordAuthentication to no.This configuration forces users to login using keys.
  * sudo nano /etc/ssh/sshd_config 
  * sudo nano /etc/ssh/ssh_config 
2. Restart ssh
  * sudo service ssh restart 
3. Disconnect and login as the grader again. This time there will be no prompt for password.

###Checking sudo access
1. Login as grader and try running any command using sudo.
  * eg: visudo 
  * If the command works fine, the graer now has sudo privileges 
2. Disable remote access to root by editing /etc/ssh/sshd_config and setting PermitRootLogin to no

##Update currently installed packages

###References
* Udacity

1.Use the following commands to update all installed packages
  * sudo apt-get update
  * sudo apt-get upgrade
  
##Change SSH port from 22 to 2200

###References
* Udacity

1. Edit the sshd_config file to change the port number.
  * sudo nano /etc/ssh/sshd_config

##Configure local timezone to UTC

###References
* [https://help.ubuntu.com/lts/serverguide/NTP.html](https://help.ubuntu.com/lts/serverguide/NTP.html)
* [http://www.pool.ntp.org/zone/us](http://www.pool.ntp.org/zone/us)

1. Install NTP to configure timezone to UTC
  * sudo apt-get install ntp
  * sudo dpkg-reconfigure tzdata
  * Note - Select the servers closest to your geographic location.
2. Edit the ntp.conf file to set the timezone
  * $ sudo nano /etc/ntp.conf
  * Edit the server information.

## Configure UFW for incoming connections

###References
* Udacity

1. Check the status of UFW. Disable if it is active.
  * sudo ufw status
  * sudo ufw disable
2. Run the following commands to configure the ports
  * sudo ufw allow 2200/ssh
  * sudo ufw allow www
  * sudo ufw allow ntp
3. Enable UFW
  * sudo ufw enable

## Install and configure Apache

###References
* Udacity
* [http://askubuntu.com/questions/454497/apache2-could-not-reliably-determine-the-servers-fully-qualified-domain-name](http://askubuntu.com/questions/454497/apache2-could-not-reliably-determine-the-servers-fully-qualified-domain-name)

1. Install Apache2.
  * sudo apt-get install apache2
2. Install mod_wsgi to serve Python appplications
  * sudo apt-get install libapache2-mod-wsgi python-dev
3. Configure Apache to handle requests by editing the default conf file.
  * sudo nano /etc/apach2/sites-enabled/000-default.conf
  * Add the following line in VirtualHost block: WSGIScriptAlias / /var/www/html/myapp.wsgi
4. Restart apache
  * sudo apache2ctl restart 
5. Create the myapp.wsgi file and add the following lines to serve the application.
  ~~~python
    def application(environ, start_response):
    status = '200 OK'
    output = 'Hello World!'

    response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
   ~~~ 
    
##Install and configure PostgreSQL
 
###References
* Udacity
* [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-0](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-0)
* [http://www.cyberciti.biz/faq/howto-add-postgresql-user-account/](http://www.cyberciti.biz/faq/howto-add-postgresql-user-account/)
* [https://www.postgresql.org/docs/9.1/static/sql-alterdatabase.html](https://www.postgresql.org/docs/9.1/static/sql-alterdatabase.html)
 
1. Install PostgreSQL
  * sudo apt-get install postgresql
2. Login using the default postgres account
  * sudo -i -u postgres
3. Go to the postgres prompt.
  * psql
4. Checked the configuration file for remote connections. By default remote connectons are disabled.
  * sudo nano /etc/postgresql/9.3/main/pg_hba.conf
5. Create a user catalog
  * CREATE USER catalog WITH PASSWORD 'pwd'
6. Create the database ItemCatalog to serve the Item Catalog app.
  * CREATE DATABASE ItemCatalog 
7. Connect to the database
  * \c ItemCatalog
8. Give permissions to the catalog user to the ItemCatalog database
  * ALTER DATABASE ItemCatalog owner catalog
9. Exit the psql prompt
  * \q
  * exit

##Install Git and cloning ItemCatalog app

###References
* [https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-14-04)
* 

1. Install Git
  * sudo apt-get install git
  * sudo apt-get install build-essential libssl-dev libcurl4-gnutls-dev libexpat1-dev gettext unzip
2. Get the app files from Git
  * wget https://github.com/anu-sha/ItemCatalog/archive/master.zip -o git.zip
3. Unzip the files and copy them to the www folder /var/www/ItemCatalog/ItemCatalog
  * unzip master.zip
4. Configure global git variables
  * git config --global user.name -name
  * git config --global user.email -email

##Configuration to run the app on the server

###References
* [https://www.howtoinstall.co/en/ubuntu/trusty/python-flask-sqlalchemy](https://www.howtoinstall.co/en/ubuntu/trusty/python-flask-sqlalchemy)
* [http://stackoverflow.com/questions/28900950/importerror-no-module-named-psycopg2](http://stackoverflow.com/questions/28900950/importerror-no-module-named-psycopg2)
* [https://www.howtoinstall.co/en/ubuntu/trusty/python-oauth2client](https://www.howtoinstall.co/en/ubuntu/trusty/python-oauth2client)

1. Install python, flask, psycopg2 and oauth2 modules
  * sudo apt-get install python-pip
  * sudo apt-get install Flask
  * sudo apt-get install python-flask-sqlalchemy
  * sudo apt-get install python-psycopg2
  * sudo apt-get install python-oauth2client
2. Configure the host by editing the init file and conference files as described above.
3. Rename the application.py to __init__.py
  * sudo nano /var/www/ItemCatalog/ItemCatalog/__init__.py
  * sudo nano /etc/apache2/sites-available/itemcatalog.conf
4. Create the wsgi file
  * sudo nano /var/www/Itemcatalog.wsgi
  ~~~ python
   #!/usr/bin/python
  import sys
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0,"/var/www/catalog/")

  from catalog import app as application
  application.secret_key = 'Add your secret key'
  ~~~
4. Edit the __init__.py file to include absolute path to client secrets file
5. Run the database_set.py file to create the database tables.
  * python database_setup.py
6. Enable the app
  * sudo a2ensite ItemCatalog
7. Make the git repository inaccessible from the browser
  * sudo nano /var/www/ItemCatalog/.htaccess
  * Add the following line to the htaccess file
    * RedirectMatch 404 /\.git
7. Browse to the site using the server ip address. If everything is setup right, the catalog app should eb displayed.
8. If there are any errors, check the error logs to find what the error is.
  * cat /var/log/apache2/error.log
9. Find the amazon ec2 hosted url for the server and add it to the allowed javascript origine=s in the google app settings.
10. Browse to the app using the amazon ec2 url.


  
  
 

  




  


