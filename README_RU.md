## Мануал по настройке VDS сервера ##

-------------------------------------------------------------------
## Оглавление
0. UFW
1. SSH
2. FTP

3. Apache
4. Nginx
5. PHP

6. Memcached
7. Mysql

8. phpmyadmin

9. Cerbot
10. Curl
11. Cron

12. Postfix
13. JQ
14. OpenSSL
15. VPN

-------------------------------------------------------------------
## Оглавление

	> sudo apt update
	> sudo nano /etc/apt/sources.list
	
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

	#deb https://packages.sury.org/php/ bullseye main
	
	#deb http://repo.mysql.com/apt/debian/ buster mysql-apt-config
	#deb http://repo.mysql.com/apt/debian/ buster mysql-5.7
	#deb http://repo.mysql.com/apt/debian/ buster mysql-tools
	# deb http://repo.mysql.com/apt/debian/ buster mysql-tools-preview
	#deb-src http://repo.mysql.com/apt/debian/ buster mysql-5.7	
	
	#deb http://ppa.launchpad.net/certbot/certbot/ubuntu lunar main
	# deb-src http://ppa.launchpad.net/certbot/certbot/ubuntu lunar main

	Получить ключ авторизации:
	> sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B7B3B788A8D3785C
	
	Если нужно добавить - удалить из репозитория:
	> sudo apt-add-repository -r ppa:certbot/certbot
	> sudo add-apt-repository --remove ppa:certbot/certbot
	
	Просмотреть репозитории:
	> lsb_release -a	
	
	Почистить удалить кэш установок:
	> sudo apt-get autoclean
	> sudo apt-cache policy
	> sudo ls -ls /etc/apt/sources.list.d/
	> sudo rm -rf /etc/apt/sources.list.d/*
	
	Обновить:
	> sudo apt update

## ################################################################
0. UFW
UFW (Uncomplicated Firewall или «простой брандмауэр») представляет 
собой интерфейс iptables, предназначенный для упрощения процесса 
настройки брандмауэра.

  0.1 Шаг:
    > sudo apt-get install ufw
    > sudo ufw enable
    > sudo ufw status
    
   Сеть:
    > sudo ss -tnlp
    > ss -lntp | sed -r 's/\t/ /g'
    > netstat
    > netstat -lnpvut (посмотрим на текущие соединения)
    
  Прочие команды: 
   > sudo ufw allow 20/tcp
   > sudo ufw deny 20/tcp
   
   > sudo ufw delete allow 20/tcp
   > sudo ufw delete deny 20/tcp

   > Открываем рабочие порты  
   > sudo ufw allow 20/tcp
   > sudo ufw allow 21/tcp 
   > sudo ufw allow 22/tcp
   > sudo ufw allow 23/tcp 


## ################################################################
1. SSH
SSH или Secure Shell - это протокол безопасного доступа из одного 
компьютера к другому по сети. 

 1.1 Шаг:
   > sudo apt install openssh-server
   > sudo systemctl enable sshd
   > ssh имя_пользователя@ip_адрес -p port
   ---------------------------------------
   > sudo groupadd usa
   > sudo adduser usa-sftp
   > sudo usermod -G usa usa-sftp
   > sudo chown usa-sftp:usa /home/hostinger/www/
   
   > правим /etc/ssh/sshd_config

	  nano /etc/ssh/sshd_config
         Include /etc/ssh/sshd_config.d/*.conf
         Port 22
		     PermitRootLogin yes
		     ChallengeResponseAuthentication no
		     UsePAM yes
		     PrintMotd no
		     AcceptEnv LANG LC_*
		     Subsystem	sftp	internal-sftp
         Match group usa-sftp
         X11Forwarding no
         ChrootDirectory /home/hostinger/www/
         ForceCommand internal-sftp
   ----------------------------------------     

  > sudo sshd -t
	> sudo systemctl restart sshd
	> sudo systemctl status sshd















