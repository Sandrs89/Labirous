
## ################################################################
7. Mysql
MySQL — a free relational database management system.
-----------------------------------------------------

  7.1 Step:
  
    > sudo apt update
    > wget https://dev.mysql.com/get/mysql-apt-config_0.8.16-1_all.deb
    > sudo apt install ./mysql-apt-config_0.8.16-1_all.deb
    
    >> p1. Debian buster
    >> p2. Mysql Server & Cluster
    >> p3. Mysql - 5.7
    >> ok
    
    > sudo apt update
    > sudo apt install -y mysql-community-server
    > sudo apt install mysql-server
    > sudo systemctl status mysql
    
    > sudo apt install mysql-server mysql-client
    > sudo systemctl enable mysqld
    > sudo systemctl is-enabled mysql.service
    > sudo systemctl enable mysql.service
    
    > adduser root mysql
    > usermod -aG sudo mysql    
    
    > sudo systemctl start mysqld
      sudo systemctl stop mysqld
    > mysql_secure_installation
    
      >> 1. n
      >> 2. n
      >> 3. y
      >> 4. y
      >> 5. y
      
      >> Remove anonymous users - delete anomoys user
      >> Disallow root login remotely - off remote control
      
      >> Remove test database and access to it - уdelete test DB
      
      >> Reload privilege tables now - restart table privelegy

	> sudo systemctl restart mysql
	> systemctl status mysql
	> sudo systemctl status mysql.service
	> mysql -u root -p

		CREATE DATABASE site1 DEFAULT CHARACTER SET utf8 DEFAULT 
		COLLATE utf8_general_ci;
		
		GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'localhost' 
		IDENTIFIED BY 'my12345' WITH GRANT OPTION;
		quit;
----------------------------------------------------------
   firewall UFW:
   
     > sudo ufw allow 3306/tcp


7.2 Step:

PHPMYADMIN представляет собой веб-приложение для управления базами 
данных MySQL и MariaDB с использованием графического пользовательского 
интерфейса (GUI).

	phpmyadmin
	> htpasswd -c /home/hostinger/phpmyadmin.htpasswd secureuser
	
	



