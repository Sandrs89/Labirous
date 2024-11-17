
## ################################################################
11. Postfix
    
Postfix - It is a free, open source mail Transfer Agent
(MTA) that routes and delivers email. 
He will be responsible for sending and receiving mail via SMTP.

-------------------------------------------------------------------

  11.1 Step:
    
11.1.1.  Install and setting Postfix:

    > sudo apt install postfix or
    > sudo DEBIAN_PRIORITY=low apt install postfix
    
    > sudo apt install postfix mailutils
    > sudo apt install postfix postfix-mysql
    
    > sudo dpkg-reconfigure postfix
      >> Internet Suite
      >> Domen (host)
       
      General type of mail configuration: (Internet site) - 
      Указываем режим, когда почта будет отправлятся и приниматься через SMTP
      
      System mail name: (mail.pip-start.ru) -
      указываем базовый домен почты, который идет после знака @
      
      Root and Postmaster recipient: (postmaster@mail.pip-start.ru) -
      учетная запись системы, которая будет получать почту на почтовые адреса root@ и postmaster@
      
      Other destinations to accept mail for: $myhostname, mail.pip-start.ru localhost -
      (Список доменов, которые сервер будет считать точкой финального назначения. Здесь можно указать другие домены, если почтовый сервер 
      будет обслуживать больше чем один домен или указать доменное имя более высокого уровня, если сервер будет работать как почтовый релей)
      
      Force synchronymous updates on mail queue: No -
      Для журнальных файловых систем, не используем синхронные обновления на очередь почты
      
      Local networks: -
      список сетей с которых будет почта будет ретранслироваться, в большинстве случаев достаточно оставить значение по умолчанию
      
      Mailbox size limit: 0 - (7500000)
      Ограничивает размер почтового ящика, 0 – без ограничений
      
      Local address extension character: + 
      Символ для разделения расширения адреса
      
      Internet protocol to use: All -
      Позволяет ограничить использование версий IP протокола, включим All – ipv4 и ipv6      
       
    > sudo systemctl enable postfix
    > sudo systemctl start postfix       
    > sudo systemctl restart postfix
    
    
12.1.2. Install patch
    
    > sudo postconf -e 'home_mailbox= Mailbox/'
    > sudo postconf -e 'virtual_alias_maps= hash:/home/vmail/virtual'

    > sudo nano /home/vmail/virtual
	   postmaster@gromo109834.localdomain root
	   admin@gromo109834.localdomain root

	> sudo postmap /home/vmail/virtual

    Edit:
	> sudo nano /etc/mailname 
	  (gromo109834.localdomain)
	  
	> cat /etc/mailname


12.1.3. System setting

    > hostname - узнаем свой хост и записываем его куда-нибудь на бумажку.
    gromo109834
    
    > sudo nano /etc/hosts
    
    # mail
    127.0.0.1 gromo109834.localdomain mail
    
    > cat /etc/hosts
	

12.1.4. Restart

    > sudo postfix reload
    > sudo systemctl restart postfix
    > sudo systemctl status postfix
    
     firewall UFW:
     > sudo ufw allow Postfix
     > sudo ufw allow 25/tcp    
    
    
 12.1.4. Database
    Create DB:
    
    > mysql --user root --password
    > CREATE DATABASE mail;
    > CREATE USER 'mail'@'localhost' IDENTIFIED BY 'pa55w0rd';
    > GRANT ALL ON mail.* TO 'mail'@'localhost';
    > exit
    > mysql mail --user mail --password pa55w0rd
    
    Create > 
		CREATE TABLE `virtual_domains` (
		`id`  INT NOT NULL AUTO_INCREMENT,
		`name` VARCHAR(50) NOT NULL,
		PRIMARY KEY (`id`)
		) ENGINE=InnoDB DEFAULT CHARSET=utf8;

		CREATE TABLE `virtual_users` (
		`id` INT NOT NULL AUTO_INCREMENT,
		`domain_id` INT NOT NULL,
		`password` VARCHAR(106) NOT NULL,
		`email` VARCHAR(120) NOT NULL,
		PRIMARY KEY (`id`),
		UNIQUE KEY `email` (`email`),
		FOREIGN KEY (domain_id) REFERENCES virtual_domains(id)
		  ON DELETE CASCADE
		) ENGINE=InnoDB DEFAULT CHARSET=utf8;

		CREATE TABLE `virtual_aliases` (
		`id` INT NOT NULL AUTO_INCREMENT,
		`domain_id` INT NOT NULL,
		`source` VARCHAR(100) NOT NULL,
		`destination` VARCHAR(100) NOT NULL,
		PRIMARY KEY (`id`),
		FOREIGN KEY (domain_id) REFERENCES virtual_domains(id)
		  ON DELETE CASCADE
		) ENGINE=InnoDB DEFAULT CHARSET=utf8;
		
   Insert >
		
  		INSERT INTO virtual_domains (`id`, `name`) VALUES (1, 'gromo109834.localdomain');

		INSERT INTO virtual_users (`id`, `domain_id`, `email`, `password`)
		  VALUES (1, 1, 'admin@gromo109834.localdomain', ENCRYPT('s3cr3t', CONCAT('$6$', SUBSTRING(SHA(RAND()), -16))));

		INSERT INTO virtual_aliases
		  (`id`, `domain_id`, `source`, `destination`) VALUES (1, 1, 'admin@gromo109834.localdomain', 'root');
    
    
12.1.5. POSTFIX
    
    > Edit nano /etc/postfix/main.cf:
    > chmod -Rf 755 /etc/postfix
    
    # max size mail 1 Мб
    message_size_limit = 1000000
    
	# настройки БД
	virtual_mailbox_domains = mysql:/etc/postfix/mysql-domains.cf
	virtual_mailbox_maps = mysql:/etc/postfix/mysql-users.cf
	virtual_alias_maps = mysql:/etc/postfix/mysql-aliases.cf
	
	
    Create connect to BD:
    ---------------
	> Create /etc/postfix/mysql-domains.cf:
	hosts = localhost
	user = mail
	password = pa55w0rd
	dbname = mail
	query = SELECT 1 FROM virtual_domains WHERE name='%s'

	> chown root:vmail /etc/postfix/mysql-domains.cf
	> sudo chmod 0644 /etc/postfix/mysql-domains.cf
    ---------------

    ---------------
	> Create /etc/postfix/mysql-users.cf:
	hosts = 127.0.0.1
	user = mail
	password = pa55w0rd
	dbname = mail
	query = SELECT 1 FROM virtual_users WHERE email='%s'

	> chown root:vmail /etc/postfix/mysql-users.cf
	> sudo chmod 0644 /etc/postfix/mysql-users.cf
    ---------------
    
    ---------------
	> Create /etc/postfix/mysql-aliases.cf:
	hosts = 127.0.0.1
	user = mail
	password = pa55w0rd
	dbname = mail
	query = SELECT destination FROM virtual_aliases WHERE source='%s' 
    
	> chown root:vmail /etc/postfix/mysql-aliases.cf
	> sudo chmod 0644 /etc/postfix/mysql-aliases.cf    
	
    ---------------
    
	> sudo chown postfix:postfix /etc/postfix/mysql-*.cf
	> sudo chmod o-rwx /etc/postfix/mysql-*.cf
    
    > echo 'export MAIL=/root/Mailbox' | sudo tee -a /etc/bash.bashrc | sudo tee -a /etc/profile.d/mail.sh
    > sudo systemctl restart postfix
    
    > sudo nano /etc/aliases
   
    
Start diagnostics

> 1. ps -ax | grep master
 
	(/usr/lib/postfix/sbin/master -w)
    
> 2. service postfix status
> 3. netstat -na  | grep LISTEN | grep  25

	(tcp        0      0 0.0.0.0:25              0.0.0.0:*               LISTEN)
    
> 4. postfix check

	> /usr/sbin/postfix -v
	> postconf | grep version
	> postconf –m
    (Ошибок не должно быть)
    
> 5. postfix stop
> 6. postfix start
    
7. Проверяем, что он видит домены, пользователей и алиасы:
   
    	> sudo postmap -q admin@gromo109834.localdomain mysql:/etc/postfix/mysql-users.cf
    	> sudo postmap -q gromo109834.localdomain mysql:/etc/postfix/mysql-domains.cf
    	> sudo postmap -q postmaster@gromo109834.localdomain mysql:/etc/postfix/mysql-aliases.cf admin@gromo109834.localdomain
    
9. Проверим порты
   
		> sudo apt install nmap
     		> nmap -v -p25,110,143,465,587,993,995 127.0.0.1
     		> netstat -lnpvut (посмотрим на текущие соединения)
     
     
10. Тестирование отправки почты

		> telnet localhost 25			      (220)
    		> HELO localhost.localdomain	              (250) 
    		> MAIL FROM:<>                                (250 2.1.0 Ok)
    		> RCPT TO:<admin@gromo109834.localdomain>     (250 2.1.5 Ok)
    		> DATA 					      (354 End data with <CR><LF>.<CR><LF>)
    		> quit
    
    		> telnet localhost 25
    		> mail from: admingromo109834.localdomain
    		> rcpt to: postmaster@gromo109834.localdomain
    		> DATA
			From: <admingromo109834.localdomain>
			To: <postmaster@gromo109834.localdomain>
			Subject: root@gromo109834.localdomain
			Hello friend mails
   
    	Чтобы завершить подключение по Telnet, введите “.” и нажмите ENTER.
    	> 250 2.0.0 Ok: queued as DE3A..28..0
    	> quit
    
    
	Справка

		MAIL FROM: <адресотправителя>
		RCPT TO: <комуписьмо>
		DATA    
    
    
11. Проверка сети
    
     	> ps -ef | grep mail
     	> dig pip-start.ru mx    
    
    
12. Тест ящика Postfix (mail)
    
    	> echo "Test mail" | mail -s "Test mail" admin@gromo109834.localdomain
    	> sudo mail -f /home/vmail/Mailbox


13. Тест ящика Postfix (s-nail)

		> sudo apt install s-nail
    		> sudo nano /etc/s-nail.rc
    
    		# разрешает клиенту открываться даже при пустом почтовом ящике
		set emptystart
	
		# указывает название каталога с структурой почтовых папок
		set folder=/home/vmail/Mailbox
	
		# создает файл sent для хранения отправленной почты
		set record=+sent    
    
   		 > echo 'Hello' | s-nail -s 'Hello' -Snorecord admin@gromo109834.localdomain
		> s-nail 
    
    
14. Debug
    
    	tail -f /var/log/mail.log    
    	tail -f /var/log/mail.err 
