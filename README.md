# MoodleInstallationSteps
Installing Moodle on Ubuntu 20.04

						Installing Pre-requisites : LAMP Stack   

sudo apt update && sudo apt upgrade -y
sudo apt install tasksel
sudo tasksel install lamp-server  	#using tasksel automates apache, mysql and php installation. 
						  	#However it may install php 8.1 instead of php 7.4. So run the following commands to install all the required php extensions

4.  sudo apt install php7.4 libapache2-mod-php7.4   php7.4-pspell php7.4-curl php7.4-gd php7.4-intl php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-ldap php7.4-zip php7.4-soap php7.4-mbstring 
5.  sudo apt install graphviz aspell ghostscript clamav php-json php-cgi php-mysql php-curl git
6.  sudo systemctl restart apache2

						  ---It is better to install lets encrypt SSL at this point(You can avoid the following steps if not required)---
7.  sudo apt install snapd
8.  sudo snap install core; sudo snap refresh core
9.  sudo ln -s /snap/bin/certbot /usr/bin/certbot
10. sudo certbot --apache           	# you provide your email id test@test.com, enter Y to agree terms and either Y or N for promotional emails, type in yourdomainame.com since we don't yet have an apche config


							#Cloning the official github repository to your local machine
11. cd /opt
12. sudo git clone git://git.moodle.org/moodle.git
13. cd moodle
14. sudo git branch -a			 	# You can find the branch code of all stable releases and choose the desired version. I am going to use the latest stable verison 400 of moodle at this time in this installation
15. sudo git branch --track MOODLE_400_STABLE origin/MOODLE_400_STABLE
16. sudo git checkout MOODLE_400_STABLE	# Switched to branch 'MOODLE_400_STABLE' Your branch is up to date with 'origin/MOODLE_400_STABLE'. - you will be prompted with this message on terminal

							

							#Copying the source code to website root directory
17. sudo cp -R /opt/moodle /var/www/html/
18. sudo chmod -R 0777 /var/www/html/moodle

19. sudo mkdir /var/moodledata
20. sudo chown -R www-data /var/moodledata
21. sudo chmod -R 0777 /var/moodledata


							#Set Up the MySQL Server
22. sudo mysql -u root -p			#You can just press enter on the password promts, or you can setup a root password. Alternatively you can do sudo mysql_secure_installation
23. CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
24. CREATE USER 'moodle-user'@'localhost' IDENTIFIED BY 'password';											#Please change the db user password to a secure password
25. GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO 'moodle-user'@'localhost';
26. quit


27. sudo vim /etc/apache2/sites-available/000-default-le-ssl.conf  (You can avoid this step if you want to browse yourdomain.com/moodle instead of yourdomain.com)
28. 	#change the line from DocumentRoot /var/www/html/ to   DocumentRoot /var/www/html/moodle  

29. sudo vim /etc/php/7.4/apache2/php.ini
30.  #Find and Uncomment the line ;max_input_vars = 1000 and change it to max_input_vars = 5000. Save and quit
31. sudo systemctl restart apache2

							#Complete the Moodle Installation
	#For step by step image reference please visit - 
	#In a web browser, navigate to the yourdomain.com. If you have not followed steps 27,28 then browse yourdomain.com/moodle for installation.
	#Follow the prompts to complete the Moodle setup. Enter the default options or your preferences for all settings except the following:
	#Enter /var/moodledata for Data directory when prompted to confirm paths.
	#Provide db username and password on the installation web interface

	#The installation may take upto 2-3 minutes.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
