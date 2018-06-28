1. Server ip and port:
   18.195.87.198  2200

2. Login to the Item Catalog application through following URL
   http://18.195.87.198.xip.io/

3. Installed package in this server:
   python 2.7
   apache2
   apache wsgi
   postgresql

4. Config:
   /etc/ssh/sshd_config
   /var/www/catalog/MyCatalog.wsgi
   /etc/apache2/sites-available/MyCatalog.conf
   
   4.1 SSH related:
	    Edit the sshd_config file with command: "$sudo vim /etc/ssh/sshd_config";
	    Search the line which contains the content "Port 22", and change it to "Port 2200";
	    Disable root remote login by change the line to "PermitRootLogin no";
	  
   4.2 UFW related:
	    Disable all port: "$sudo ufw default deny";
	    Enable http, ntp, ssh port using following commands:
	      $sudo ufw allow http
		   $sudo ufw allow ntp
		   $sudo ufw allow 2200/tcp
	    Enable ufw:
	      $sudo ufw enable
		  
   4.3 User grader relted:
         $sudo adduser grader
	      $sudo usermod -aG sudo grader
	      Switch to user grader and config public/private key:
	        $ssh-keygen
		   Enter into folder .ssh and run command
		     $cat id_rsa.pub >> authorized_keys
			 
   4.4 Postgresql related:
         $apt install postgresql
	      $sudo -u postgres psql
	      Inside psql environment excute:
	         CREATE DATABASE catalogdb;
            CREATE USER catalog WITH ENCRYPTED PASSWORD 'password';
            GRANT ALL PRIVILEGES ON DATABASE catalogdb TO catalog;
		   
   4.5 Catalog project deploy related:
         $apt install git
	      $cd /var/www
	      $mkdir catalog
	      Download MyCatalog folder from git and put it into this newly created catalog folder;
	      Create and config MyCatalog.wsgi
	      Create and config MyCatalog.conf under folder /etc/apache2/sites-available;
	      $cd /etc/apache2/sites-enabled
	      Create soft link to MyCatalog.conf in /etc/apache2/sites-enabled:
	         $ln -s MyCatalog.conf /etc/apache2/sites-available
	      Try to access the site, use "sudo cat /var/log/apache2/errors.log" to check error log.
	
   
5. User and password:
   grader/thisisapass
   
6. Third-party resources used in this project:
   firewall:
   https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server
   flask:
   http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/

7. Find the SSH Key in grader.pem
