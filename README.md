This file include all instructions and clear explanations of the VPS setup on digital ocean,
 LEMP installation and configurations :


•	Steps to setup ubantu VPS on digital_Ocean
•	For creating a VPS, I will choose DigitalOcean.

1. Click on create and then on droplets
2. Choose the droplet, Operating system, and plan size.
3. Choose a region. Ideally, this has to be the region closest to your users
4. Choose a password and click create droplet
5. We will get a public IP address
6. We will use this public IP address to login into our server

Step to login to server :

•	The very first step to start using a server is to log in to it. 
•	We will use an SSH Client to get access to the server's terminal (this remote computer), and once we have access to the terminal, we can start executing commands on this server. 
open powershell and execute following commands:
----ssh root@your_server_ip------
-----Password----

Creating a non-root user :

•	We will now create a non-root user account that will have restricted access to the machine. This is ideal if you want to have people working for you on the server without doing everything on the server.

command: ----adduser username-------

•	We have successfully created an account with basic access. We want this user to be able to elevate to root access when required. This will be useful when you want to use your server and sometimes do something which requires root access.

command : ------usermod -aG sudo user name-------


Firewall setup :

•	Our server is public, and we want to protect it from external unwanted connections. We want only selected services exposed to the public on our server.

•	We will set up the UFW firewall on our Ubuntu 20.04 server and allow OpenSSH, which allows us to connect to our server.

command : ------ufw allow OpenSSH-----

Enable the firewall by executing the following command:

command: -------ufw enable-----

•	You can type the following command to elevate to the root access as and when required

command: ----------sudo su------

-------------------------------------------------------------------------------------------------------

Steps to install LEMP stack (Linux, Nginx, MySQL, PHP) on Ubuntu server :

•	we will install the LEMP stack on the ubuntu 20 based server. LEMP consists of Linux, Nginx(pronounced as Engine-x), MySQL for database, and PHP as a backend programming language. 

Step 1 - Update the server's package index
command: ------- sudo apt update -------

Step 2 - Install Nginx :
command:  -------sudo apt install nginx-------	

Step 3 - Allow Nginx through the firewall
command: -------sudo ufw app list-------
command: -------sudo ufw allow 'Nginx Full'----

Nginx install succesfully 

Step 4 - Installing MySQL

command: ------sudo apt install mysql-server----

•	This command will install MySQL, and you will be able to see the console by entering "sudo mysql" in the terminal.

command: -----sudo mysql-------

Step 5 - Installing PHP

command:  ------sudo apt install php-fpm php-mysql-----

•	If you want to host a PHP website, you will have to copy your files to '/var/www/html' and modify the file located at '/etc/nginx/sites-available/default' to look something like this.

•	these are the lines added inside the "location ~ \.php$ {" block

include fastcgi_params;
fastcgi_index index.php;
fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;

Here is a sample index.php site for you to try out:

<?php
phpinfo();
?>

You will now be able to use PHP with Nginx

--------------------------------------------------------------------

Steps to create CI-CD pipeline using Git hub actions :



Step 1 : create a repository for your project in git-hub.

Step 2 : Clone the repository to your local repository.

Step 3: Go to setting then go to secret  ---> Actions ---> Repository secrets.

Step 4 : Create secrets for HOST with host ip address.

Step 5: Create user name as root because we will be connecting with root to server.

Step 6 : create SSH private key, for creating ssh key visit your linux / ubuntu server.

Step 7 : Generate SSH key by using command: ssh-keygen -t rsa -b 4096 -t "email address.

Step 8 : Once you get SSH key create SSH_PRIVATE_KEY in your actions secret.

Step 9 : Create directory in server.

Step 10 : Clone your repository in newly created directory.

Step 11: Go to actions tab click on steup a workflow yourself , write yaml file for CI_CD pipeline.

Step 12 :Commit changes made in your yml code for CI_CD pipeline.

Step 13 :Watch CI_CD flow in action & if any error accure debug it.

Step 14 : Test the work-flow by making changes in local repo and check if its reflecting on server.

--------------------------------------------------------------------------------------------------------------------------------------------------------


Steps to create SSL/TLS certificate for secure communication
between the server and clients.

Step 1 : 
Prerequisites:
•	A registered domain name pointing to your server's IP address.
•	Root or administrative access to your server.

step 2 :
   Install Certbot
   Certbot is a command-line tool provided by Let's Encrypt that simplifies the process of obtaining and managing SSL certificates.
  commands :sudo apt update
           sudo apt install certbot python3-certbot-nginx
step 3 :
   Obtain a Certificate:
  command : sudo certbot --nginx
step 4:
   Testing:
   Test your website after enabling SSL/TLS to ensure everything is working as expected. Make sure the padlock icon appears in the
   browser's address bar.   
 
NOTE: If this does not work for creating certificate you can use other methods for creating ssl/tls certificates but its is less secure 
     
Step to get self sign certificates for SSL:

Step 1 : Generate a RSA private key 
  command : opensslgenrsa -des3 -out myhostname.key 4096
Step 2: Set pass phrase for myhostname.key
Step 3 : creating a signing request
  command :openssl req -new -key myhost.key -out myhostname.csr
    enter pass phrase and fill deatils
Step 4 : Copy key 
  command : cp myhostname.key myhostname.key.pw
Step 5 :
   command: cp openssl rsa -in myhostname.key.pw -out myhostname.key
Step 6 :
  cat myhostname.csr
Step 7 : Requesting to sign certificate
  command : openssl x509 -req -in myhostname.csr -signkey myhostname.key -out myhostname.crt
Step 8 : create a directory to save certificate 
   command : sudo mkdir /etc/nginx/ssl
Step 9 :
  command : sudo cp myhostname.crt /etc/nginx/ssl
  command : sudo cp myhostname.key /etc/nginx/ssl
Step 10 :
     edit nginx confg file and enable server 
     restart nginx
