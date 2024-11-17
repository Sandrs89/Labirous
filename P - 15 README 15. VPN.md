
## ################################################################
15.VPN Server
	
	1. Установка пакетов сервера:
	> sudo apt update
	> sudo apt install wireguard	
	> sudo apt install iptables
	> sudo apt install ipcalc
	> sudo apt install traceroute
	> sudo apt install dnsutils
	> sudo apt install openresolv
	
	> sudo systemctl status systemd-resolved.service
	> sudo systemctl enable systemd-resolved.service
	> sudo systemctl start systemd-resolved.service
	> sudo systemctl restart wg-quick@wg0-client.service
	> networkctl status wg0
	
	
	> sudo systemctl deamon-reload
	> sudo systemctl start systemmd-networkd
	
	
	2. Фаервол:
	> sudo ufw allow 22/tcp
	> sudo ufw allow 53/tcp
	> sudo ufw allow 443/tcp
	> sudo ufw allow 80/tcp
	> sudo ufw allow 51820/udp
	> sudo ufw status verbose 
	> sudo ufw status	
	
	
	3. Создаем каталог и права:
	> sudo mkdir -p /etc/wireguard
	> sudo chmod 0777 /etc/wireguard
	> sudo ls -ls /etc/wireguard
	
	
	4. Генерируем ключи (клиент-сервер):
	> cd /etc/wireguard
	
	Сервер:
	> sudo wg genkey | tee server-private.key | wg pubkey > server-public.key
	
	Смотрим публичный ключ сервера
	> sudo cat /etc/wireguard/server-private.key | wg pubkey | sudo tee /etc/wireguard/server-public.key
	
	Клиент:
	> sudo wg genkey | tee client-private.key | wg pubkey > client-public.key
	
	Смотрим публичный ключ клиента
	> sudo cat /etc/wireguard/client-private.key | wg pubkey | sudo tee /etc/wireguard/client-public.key
	
	Права:
	> sudo chmod 600 /etc/wireguard/*-private.key
	> sudo ls -ls /etc/wireguard
	
		
	5. Создаем конфигурацию сетевой карты
	> sudo nano /etc/wireguard/wg0-vpn.conf
	
	5.1. Проверяем интерфейс карты для iptables:
	> ip route list default
		# default via 91.215.152.1 dev eth0 onlink
		# После слова dev запоминаем сетевой интерфейс (eth0)
		
	5.2. Копируем ключ сервера №1
	> sudo cat /etc/wireguard/server-private.key
	
	5.3. Копируем ключ клиента №1
	> sudo cat /etc/wireguard/client-public.key
	
	[Interface]
	  PrivateKey = Вставляем ключ сервера №1
	  Address = 10.0.0.1/24
	  ListenPort = 51820
	  SaveConfig = true
	  
	  PostUp = iptables -A FORWARD -i wg0-vpn -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE; ip6tables -A FORWARD -i wg0-vpn -j ACCEPT;  ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
	  PostDown = iptables -D FORWARD -i wg0-vpn -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE; ip6tables -D FORWARD -i wg0-vpn -j ACCEPT; ip6tables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

	  # Nat Interface
	  #PostUp = ufw route allow in on wg0-vpn out on eth0
	  #PostUp = iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
	  #PostUp = ip6tables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
	  
	  #PreDown = ufw route delete allow in on wg0-vpn out on eth0
	  #PreDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
	  #PreDown = ip6tables -t nat -D POSTROUTING -o eth0 -j MASQUERADE	  
	
	[Peer]
	PublicKey = Вставляем ключ клиента №1
	AllowedIPs = 10.0.0.0/24
	Endpoint = внешний_айпи_сервера 91.215.152.114:51820
	PersistentKeepalive = 20
		

	6. # Настройка сети WireGuard:
	# Этот раздел не нужен для внутреней сети.
	# Если перенаправлять интернет-трафик через сервер WireGuard, 
	# тогда:
	> # sudo nano /etc/sysctl.conf
		# net.ipv4.ip_forward = 1
		# net.ipv6.conf.all.forwarding=1
		
	> # sudo sysctl -p
	# Результат получаем:
		# net.ipv4.ip_forward = 1
		# net.ipv6.conf.all.forwarding=1	
	
	
	7. Запуск VPN	
	> sudo systemctl enable wg-quick@wg0-vpn
	> sudo systemctl enable wg-quick@wg0-vpn.service
	
	> sudo systemctl start wg-quick@wg0-vpn
	> sudo systemctl start wg-quick@wg0-vpn.service
	
	> sudo systemctl status wg-quick@wg0-vpn
	> sudo systemctl status wg-quick@wg0-vpn.service

	Если есть ошибки:
	> sudo systemctl status wg-quick@wg0-vpn.service
			
	
	8. Проверка:
	Проверяем wireguard:
	> lsmod|grep wireguard

	Проверяем сеть:
	> ip a show wg0-vpn
	> ip addr show dev wg0
	> sudo wg show
	Если все настроено правильно:
	-----------------------------------
		interface: wg0-vpn
		  public key: tWMKRpFwXDIzq+y7mHH7GoI0NMeTzb+HcL6pCaKukzs=
		  private key: (hidden)
		  listening port: 51820

		peer: FeHnBs9uwegJEb6fouPaImqS8uTyh2ZYnea9h9bSJTo=
		  endpoint: 91.215.152.114:51820
		  allowed ips: 10.0.0.0/24
		  transfer: 0 B received, 12.00 KiB sent
		  persistent keepalive: every 20 seconds
	-----------------------------------	
	
	> ip a	
	Здесь должны увидеть wg0-vpn интерфейс
	
	Проверяем внешний IP сервера
	> curl ifconfig.me 
	ipv6: 2001:67c:2f4c:2::562 (Работает правильно)	
	
	> sudo watch wg show (ключи должны совпадать с):

	> sudo cat /etc/wireguard/server-public.key
	> sudo cat /etc/wireguard/client-public.key
	
	
	9. Прочее:
	Включаем конфигурацию:
	> sudo wg-quick up wg0-vpn (только для проверки, не совместим с systemctl start)
	> ip addr show wg0-vpn
	
	Отключаем конфигурацию:
	> sudo wg-quick down wg0-vpn
	> sudo systemctl stop wg-quick@wg0-vpn
	> sudo systemctl stop wg-quick@wg0-vpn.service
	> Повторить пункт 4 и затем пункт 7.
	
	Удаление директории с конфигами
	> rm -rf ~/wireguard	
	
	
## ################################################################
15.VPN Client
	
	1. Установка пакетов клиента:
	> sudo apt update
	> sudo apt install wireguard	
	> sudo apt install iptables
	> sudo apt install ipcalc
	> sudo apt install traceroute
	> sudo apt install dnsutils
	
	
	2. Фаервол:
	> sudo ufw allow 22/tcp
	> sudo ufw allow 53/tcp
	> sudo ufw allow 443/tcp
	> sudo ufw allow 80/tcp
	> sudo ufw allow 51820/udp
	> sudo ufw status verbose 
	> sudo ufw status	
	
	
	3. Создаем каталог и права:
	> sudo mkdir -p /etc/wireguard
	> sudo chmod 0777 /etc/wireguard
	> sudo ls -ls /etc/wireguard
	
	
	4. Создаем конфигурацию сетевой карты
	> sudo nano /etc/wireguard/wg0-client.conf
	
	4.1. Копируем ключ сервера №2
	> sudo cat /etc/wireguard/server-public.key
	
	4.2. Копируем ключ клиента №2
	> sudo cat /etc/wireguard/client-private.key	
	
	[Interface]
	Address = 10.0.0.2/24
	PrivateKey = Вставляем ключ клиента №2
	#DNS = 91.215.152.1 # 77.88.8.8

	#PostUp = ip rule add table 200 from 91.215.152.114
	#PostUp = ip route add table 200 default via 91.215.152.1

	#PreDown = ip rule delete table 200 from 91.215.152.114
	#PreDown = ip route delete table 200 default via 91.215.152.1

	[Peer]
	PublicKey = Вставляем ключ сервера №2
	AllowedIPs = 10.0.0.0/24
	Endpoint = 91.215.152.114:51820
	PersistentKeepalive = 20


	5. Запуск VPN	
	> sudo systemctl enable wg-quick@wg0-client
	> sudo systemctl enable wg-quick@wg0-client.service
	
	> sudo systemctl start wg-quick@wg0-client
	> sudo systemctl start wg-quick@wg0-client.service
	
	> sudo systemctl status wg-quick@wg0-client
	> sudo systemctl status wg-quick@wg0-client.service

	Если есть ошибки:
	> sudo systemctl status wg-quick@wg0-client.service


	6. Проверка:
	Проверяем wireguard:
	> lsmod|grep wireguard	
	
	Проверяем сеть:
	> ip a show wg0-client

	> ip a	
	Здесь должны увидеть wg0-client интерфейс	
	
	> sudo wg show
	Если все настроено правильно:
	-----------------------------------
		interface: wg0-client
		  public key: tWMKRpFwXDIzq+y7mHH7GoI0NMeTzb+HcL6pCaKukzs=
		  private key: (hidden)
		  listening port: 51820

		peer: FeHnBs9uwegJEb6fouPaImqS8uTyh2ZYnea9h9bSJTo=
		  endpoint: 91.215.152.114:51820
		  allowed ips: 10.0.0.0/24
		  transfer: 0 B received, 12.00 KiB sent
		  persistent keepalive: every 20 seconds
	-----------------------------------			
	

	7. Прочее:
	Включаем конфигурацию:
	> sudo wg-quick up wg0-client (только для проверки, не совместим с systemctl start)
	> ip addr show wg0-client
	
	Отключаем конфигурацию:
	> sudo wg-quick down wg0-client
	> sudo systemctl stop wg-quick@wg0-client
	> sudo systemctl stop wg-quick@wg0-client.service
	> Повторить пункт 4 и затем пункт 7.

	Для удаления vpn:
	> sudo apt remove wireguard 
	> sudo apt autoclean && sudo apt autoremove
	
