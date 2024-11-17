-------------------------------------------------------------------
## General system

	> sudo apt update
	> sudo nano /etc/apt/sources.list

  # deb [Debian GNU/Linux 11.0.0 _Bullseye

  # Package update system the Unix
	deb http://deb.debian.org/debian/ bullseye main
	deb-src http://deb.debian.org/debian/ bullseye main

	deb http://deb.debian.org/debian bullseye-updates main
	deb-src http://deb.debian.org/debian bullseye-updates main

	deb http://ftp.debian.org/debian bullseye main contrib non-free
	deb-src http://ftp.debian.org/debian bullseye main contrib non-free

	deb http://ftp.debian.org/debian bullseye-updates main contrib non-free
	deb-src http://ftp.debian.org/debian bullseye-updates main contrib non-free

	deb http://security.debian.org/debian-security bullseye-security main
	deb-src http://security.debian.org/debian-security bullseye-security main

  # Package update system the PHP, MySQL
	#deb https://packages.sury.org/php/ bullseye main
	
	#deb http://repo.mysql.com/apt/debian/ buster mysql-apt-config
	#deb http://repo.mysql.com/apt/debian/ buster mysql-5.7
	#deb http://repo.mysql.com/apt/debian/ buster mysql-tools
	# deb http://repo.mysql.com/apt/debian/ buster mysql-tools-preview
	#deb-src http://repo.mysql.com/apt/debian/ buster mysql-5.7	

	Get key auth package:
	> sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B7B3B788A8D3785C

  # Package update the Certbot
	#deb http://ppa.launchpad.net/certbot/certbot/ubuntu lunar main
	# deb-src http://ppa.launchpad.net/certbot/certbot/ubuntu lunar main
	
	Other: If add - dell from repository:
	> sudo apt-add-repository -r ppa:certbot/certbot
	> sudo add-apt-repository --remove ppa:certbot/certbot
	
	View repository:
	> lsb_release -a	
	
	Clear or delete cache install:
	> sudo ls -ls /etc/apt/sources.list.d/
	> sudo rm -rf /etc/apt/sources.list.d/*
	
	Update:
	> sudo apt update
