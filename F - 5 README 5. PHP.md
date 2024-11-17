
## ################################################################
5. PHP
   
		PHP (recursive acronym of the PHP phrase: Hypertext Preprocessor)
		is a widespread open source general purpose programming language.

------------------------------------------
 5.1 Step:
 
   	> echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
   	> sudo apt-get update
   	> sudo rm -rf /etc/apt/trusted.gpg.d/php.gpg
   	> sudo apt-key del B188E2B695BD4743
   	> sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
   	> sudo apt-get update

 ------------------------------------------
is install php 8.0 to next:

	> sudo apt-get update
	> sudo update-alternatives --set php /usr/bin/php7.4

------------------------------------------
	> sudo apt-get install php7.4
	> sudo apt install php php-fpm
	> sudo apt install php php7.4-fpm
	> sudo systemctl enable php7.4-fpm
   
	> sudo apt install php-xml php-gd php-curl php-zip php-mbstring 
	> sudo apt install php-bz2 php-cgi php-common php-imap php-json 
	> sudo apt install php7.4-odbc php7.4-opcache php7.4-xmlrpc php7.4-xsl php7.4-fpm
	> sudo apt install  php7.4-curl php7.4-mysql  php7.4-memcached
	> sudo apt install php7.4-gd php7.4-mysqli
	> sudo apt install php-imap

	> sudo apt-get -y install libmcrypt-dev

------------------------------------------
	> a2enmod setenvif
	> a2enmod php7.4
	> a2enmod mpm_prefork

------------------------------------------
	> edit /etc/php/7.4 (in catalog php.ini)
	nano /etc/php/7.4/*   

------------------------------------------
	> sudo systemctl restart php7.4-fpm
	> sudo systemctl status php7.4-fpm
	> php -v
