


## ################################################################
Прочие инструкции unix:


  Кодировка:
   переформатирование файла
   > sudo apt install dos2unix
   > sudo dos2unix  /home/myfile.sh
   

  Каталоги:
   удалить католог принудительно 
   > sudo rm -rf  /home/news
  
   
  Права:
  смена прав на папку
  > sudo chmod a-w /home/hostinger/ftp
  > sudo ls -la /home/hostinger/ftp  
  
  
  Пользователи:
	 сменить пароль пользователя
	 > passwd root

	 удалить пользователя
	 > deluser root

	 добавить пользователя
	 > adduser root


  Ссылки: 
   создать ссылку
   > ln -s /path_to_folder /home/hostinger/ftp
   > ln -sf /path_to_file /home/hostinger/ftp

	/path_to_folder       <-- на что будем указывать
	/home/hostinger/ftp   <-- папка в которой создадим ссылку


	hosts
	192.168.48.92		test.example.su
	
###################################################### SETTING LAN

	> Вызываем cmd win (DNS)
	nslookup 192.168.1.44

	> создаем файл с сетевыми настройками
	nano /etc/network/interfaces.d/eno1

	> пишем туда настройки
	auto eno1
	iface eno1 inet static

	address 192.168.1.252
	netmask 255.255.255.0
	gateway 192.168.1.1

	dns-nameservers 192.168.1.1
	mtu 1500

	> перезагружаем сеть
	systemctl restart network


	> меняем или создаем DNS на сервере
	nano /etc/resolv.conf

	> пишем настройки DNS
	nameserver 192.168.1.1
	nameserver 192.168.1.1
	
	> перезагружаем DNS и смотрим статус
	sudo systemctl restart systemd-resolved.service
	sudo systemctl status systemd-resolved.service

	> только по необходимости (запрещаем перезаписывать)
	sudo ln -sf /dev/null /etc/resolv.conf


