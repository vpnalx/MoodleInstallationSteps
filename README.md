# MoodleInstallationSteps
Installing Moodle on Ubuntu 20.04

Installing Pre-requisites : LAMP Stack   

	sudo apt update && sudo apt upgrade -y
	sudo apt install tasksel
	sudo tasksel install lamp-server

	sudo apt install php7.4 libapache2-mod-php7.4 php7.4-pspell php7.4-curl php7.4-gd php7.4-intl php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-ldap php7.4-zip php7.4-soap php7.4-mbstring 
	sudo apt install graphviz aspell ghostscript clamav php-json php-cgi php-mysql php-curl git
	sudo systemctl restart apache2

	sudo apt install snapd
	sudo snap install core; sudo snap refresh core
	sudo ln -s /snap/bin/certbot /usr/bin/certbot
	sudo certbot --apache           	
	
	cd /opt
	sudo git clone git://git.moodle.org/moodle.git
	cd moodle
	sudo git branch -a
	sudo git branch --track MOODLE_400_STABLE origin/MOODLE_400_STABLE
	sudo git checkout MOODLE_400_STABLE

	sudo cp -R /opt/moodle /var/www/html/
	sudo chmod -R 0777 /var/www/html/moodle

	sudo mkdir /var/moodledata
	sudo chown -R www-data /var/moodledata
	sudo chmod -R 0777 /var/moodledata
						
	sudo mysql -u root -p
	mysql>CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
	mysql>CREATE USER 'moodle-user'@'localhost' IDENTIFIED BY 'password';
	mysql>GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO 'moodle-user'@'localhost';
	
	sudo vim /etc/php/7.4/apache2/php.ini
	##Find and Uncomment the line ;max_input_vars = 1000 and change it to max_input_vars = 5000. Save and quit## You can also change the upload max size here##
	sudo systemctl restart apache2

							#Complete the Moodle Installation, browse yourdomain.com/moodle for installation.
