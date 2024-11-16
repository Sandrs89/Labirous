
## ################################################################
3. Apache
Apache - this is a service running in the background. One of the web servers. 

   3.1 Step:
	 > sudo apt install apache2	
	 > sudo systemctl enable apache2
	 > sudo systemctl start apache2
	 
	 > edit /etc/apache2/apache2.conf
	   nano /etc/apache2/apache2.conf
	   
	 > edit /etc/apache2/ports.conf
	   nano /etc/apache2/ports.conf
	   	 
	 > /etc/apache2/sites-available/000-default.conf
	   nano /etc/apache2/sites-available/000-default.conf
	   
	 > /etc/apache2/sites-available/default-ssl.conf
	   nano /etc/apache2/sites-available/default-ssl.conf   
	   
	 > apachectl configtest
	 > sudo apachectl -t
	 > sudo systemctl reload apache2
	 > sudo systemctl restart apache2
	 > ls /etc/apache2/mods-enabled
	 
   > a2enmod rewrite
	 > a2enmod http2
	 > a2enmod headers
	 > a2enmod expires
	 
	 Отключение модуля Apache: 
	 > a2dismod 
	 
	 
	 > a2ensite test.com (Только по необходимости "Подключаем виртуальный хост")
	 > sudo systemctl restart apache2

   Если есть firewall UFW:
     > sudo ufw allow 80/tcp
