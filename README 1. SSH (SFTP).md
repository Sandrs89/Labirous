## ################################################################
# 1. SSH (SFTP)
SSH или Secure Shell - это протокол безопасного доступа из одного 
компьютера к другому по сети. 

 ## 1.1 Шаг:
      > sudo apt install openssh-server
      > sudo systemctl enable sshd
      > ssh имя_пользователя@ip_адрес -p port
   ---------------------------------------
      > sudo groupadd usa
      > sudo adduser usa-sftp
      > sudo usermod -G usa usa-sftp
      > sudo chown usa-sftp:usa /
      > usermod -aG sudo blg-sftp
   
      > правим /etc/ssh/sshd_config
        nano /etc/ssh/sshd_config
	
        Include /etc/ssh/sshd_config.d/*.conf
	
	      # Локальный адрес, порт  на которых сервер sshd будет принимать соединения.
	      ListenAddress 91.215.152.114
	      Port 22
	
	      # Время ожидания регистрации пользователю в системе.
	      LoginGraceTime 30
	
	      # Число одновременных соединений, в которых еще не пройдена аутентификация.
	      MaxStartups 5
	
	      # Количество попыток входа в систему в рамках сеанса связи.
	      MaxAuthTries 2
	
	      # логгирования событий
	      LogLevel ERROR

	      # Разрешить аутентификацию по открытому ключу. Только протокол v.2.
	      PubkeyAuthentication yes
	
	      # разрешить пользователю root вход через протокол SSH
	      PermitRootLogin no
	
	      # Разрешить аутентификацию по пользователь & пароль.
	      PasswordAuthentication yes
	
	      # Разрешить использование пустых паролей при аутентификации по паролю.
	      PermitEmptyPasswords no
	
	      # Разрешена ли беcпарольная аутентификация "запрос-ответ".
	      ChallengeResponseAuthentication no
	
	      # Включить дополнительные, подключаемые модули аутентификации  
	      UsePAM yes
	
	      # Использовать DNS запросы, с целью проверки
	      UseDNS yes
	
	      # Показывать содержимое файла /etc/motd при входе пользователя
	      PrintMotd no
	
	# Переменных среды, принимаемые от клиента
	AcceptEnv LANG LC_*
	
	# Время простоя клиента в секундах
	ClientAliveInterval 20
	
	# Количество проверок доступности клиента, которые могут оставаться без ответа.
	ClientAliveCountMax 3
	
	# Данная директива позволяет разрешить сжатие
	Compression yes
	
	# Включает внешнюю подсистему 
	Subsystem sftp internal-sftp
	
	 # Директива образует блок
	 Match group blg
	
	 # Разрешить туннелирование X11.
	 X11Forwarding no

	 # Директива открываемого каталога после авторизации
	 ChrootDirectory /
	
	 # В данной директиве можно указать команду, которая будет выполнена 
	 # при входе пользователя в систему
	 #ForceCommand internal-sftp
   ----------------------------------------     

   > правим /etc/ssh/ssh_config
     nano /etc/ssh/ssh_config
	Include /etc/ssh/ssh_config.d/*.conf
	Host *
	    HashKnownHosts yes
	    GSSAPIAuthentication yes
	    ServerAliveInterval 60
	    ServerAliveCountMax 5

   ----------------------------------------

   > правим /etc/motd
     nano /etc/motd
	===========================
	-- Welcome to USA Server --
	===========================

  > sudo sshd -t
  > sudo systemctl restart sshd
  > sudo systemctl status sshd
