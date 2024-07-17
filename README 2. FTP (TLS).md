## ################################################################
# 2. FTP
File Transfer Protocol, т. е. FTP – протокол передачи файлов и, как 
понятно из полного названия, предназначен для передачи файлов между 
удалёнными компьютерами через сеть. 

 # 2.1 Шаг:
    > sudo apt-get install proftpd
    > sudo systemctl enable proftpd

--------------------------------------------

   # Создаем каталоги:
    > sudo mkdir /home/hostinger && sudo chmod 0777 /home/hostinger
    > sudo mkdir /home/hostinger/bin-tmp && sudo chmod 0777 /home/hostinger/bin-tmp
    
    > sudo mkdir /home/hostinger/ftp && sudo chmod 0777 /home/hostinger/ftp
    > sudo mkdir /home/hostinger/www && sudo chmod 0777 /home/hostinger/www
    
    > sudo mkdir /home/hostinger/log && sudo chmod 0777 /home/hostinger/log
    > sudo mkdir /home/hostinger/log/domain && sudo chmod 0777 /home/hostinger/log/domain
    
    > sudo mkdir /home/hostinger/log/php && sudo chmod 0777 /home/hostinger/log/php
    > sudo mkdir /home/hostinger/log/php/sessions && sudo chmod 0777 /home/hostinger/log/php/sessions
    
    > sudo mkdir /home/hostinger/log/cron_log && sudo chmod 0777 /home/hostinger/log/cron_log

    > sudo mkdir /home/server && sudo chmod 0777 /home/server
    > sudo mkdir /home/server/command && sudo chmod 0777 /home/server/command
    > sudo mkdir /home/server/cron && sudo chmod 0777 /home/server/cron
    > sudo mkdir /home/server/notify && sudo chmod 0777 /home/server/notify
    > sudo mkdir /home/server/updateSSl && sudo chmod 0777 /home/server/updateSSL
    
    > sudo adduser usa-ftp
    > sudo usermod -G usa usa-ftp
    > sudo chown usa-ftp:usa /home/hostinger/www/

    --------------------------------------------
   ## > правим /etc/proftpd/proftpd.conf 
      sudo nano /etc/proftpd/proftpd.conf 

      # /etc/proftpd/proftpd.conf -- This is a basic ProFTPD configuration file.

      Include /etc/proftpd/modules.conf

      # Указываем пользователя/группу от имени которых будет запущен ProFTPD
      User usa-ftp
      Group usa

      # Запираем всех в домашнем каталоге
      DefaultRoot /home

      # запрещаем подключаться от пользователя root
      RootLogin off

      # Указываем IP-адрес & порт на каком будет работать ftp
      DefaultAddress  91.215.152.114
      Port 21

      # разрешить пассивную передачу данных на IP
      MasqueradeAddress       91.215.152.114

      # Не использовать протокол IPv6 
      UseIPv6 off

      # Указываем диапазон портов
      PassivePorts 40000 65535

      # имя сервера, которое высвечивается при подключении
      ServerName "Bolgarian"

      # режим работы - автономный
      ServerType standalone

      # показывать приветственное сообщение
      DeferWelcome on

      # делаем сервером по умолчанию
      DefaultServer on

      # Разрешить ходить по ссылка
      ShowSymlinks on

      # Таймауты
      TimeoutNoTransfer 360
      TimeoutStalled 600
      TimeoutIdle 180
      TimeoutLogin 20

      # Выводимое сообщение
      AccessGrantMsg  "Hello to Bolgarian Server"

      # Максимальное кол-во процессов
      MaxInstances    8

      # Общее максимальное число соединений
      MaxClients 3

      # максимальное количество подключений
      MaxCLientsPerUser 8
      MaxHostsPerUser 8

      # Максимальное число соединений с одного хоста
      MaxClientsPerHost 8 "%m current connect, new denied client"

      # Максимальное число попыток ввода пароля
      MaxLoginAttempts 3 "current close sign"

      # Маска для назначения прав при создание файлов
      Umask   023     023

      # Обратный поиск данных IP-адресов
      UseReverseDns on

      # Разрешить соединения на основе /etc/shells
      RequireValidShell       off

      # Запретить пересылку сервер-сервер
      AllowForeignAddress off

      # Директива отвечающая за .ftaccess файлы
      AllowOverride off

      # Директива позволяющая переписывать файлы
      AllowOverwrite on

      # выдавать многострочные сообщения в стандарте 
      MultilineRFC2228 on

      # Сообщение после успешного захода на сервер
      AccessGrantMsg "Welcome to Bolgarian Server"

      # переключение поиска идентификаторов 
      <IfModule mod_ident.c>
        IdentLookups off
      </IfModule>

      # Показывать содержимое каталога
      ListOptions "-l"

      # регулярное выражение аргументов команды, которые нужно заблокировать 
      #DenyFilter \*.*/

      # Паттерн для проверки комманд отправляемых от клиента-серверу
      #AllowFilter ^[-A-Za-z0-9_.(),/]*$

      # Разрешить только "az 0-9" . - _ в именах файлов с символами верхнего регистра
      #PathAllowFilter ^[A-Za-z0-9._-]+$

      # позволяем продолжать скачивания и закачки
      AllowStoreRestart on
      AllowRetrieveRestart on

      # удаляем незавершенные закачки
      DeleteAbortedStores on

      # SECURITY VIOLATION: Passive connection
      AllowForeignAddress on

      # Маска изменяющая права доступа
      <Directory /home/*>
        Umask 023
        AllowOverwrite on
          <Limit MKD STOR DELE XMKD RNEF RNTO RMD XRMD CWD>
            AllowAll
          </Limit>

          # Разрешение на смену прав файлам и создание каталогов
         <Limit ALL SITE_CHMOD>
           AllowAll
         </Limit>

         # разрешим READ / WRITE
         <Limit READ WRITE>
           AllowAll
         </Limit>
       </Directory>

       # использовать PAM-аутентификацию
       AuthPAM off

       # маскировка адресов с динамическими IP-адресами
       <IfModule mod_dynmasq.c>
         DynMasqRefresh 28800
       </IfModule>

       # Логи
       TransferLog     /var/log/proftpd/xferlog.log
       ExtendedLog     /var/log/proftp/extended.log
       SystemLog       /var/log/proftpd/proftpd.log

       <IfModule mod_quotatab.c>
         QuotaEngine off
       </IfModule>

       <IfModule mod_ratio.c>
        Ratios off
       </IfModule>

       # Защита от временной аттаки 
       <IfModule mod_delay.c>
          DelayEngine on
       </IfModule>

       <IfModule mod_ctrls.c>
          ControlsEngine off

          ControlsMaxClients 3
          ControlsLog /var/log/proftpd/controls.log

          ControlsInterval 5
          ControlsSocket /var/run/proftpd/proftpd.sock
       </IfModule>

       #
       <IfModule mod_ctrls_admin.c>
          AdminControlsEngine off
       </IfModule>

       # Это используется для соединений по протоколу FTPS
       Include /etc/proftpd/tls.conf

       # Разделение директив VirtualHost/Виртуальный хост
       #Include /etc/proftpd/virtuals.conf

       Include /etc/proftpd/conf.d/

--------------------------------------------
 



  
     


      > sudo systemctl start proftpd
      > sudo systemctl restart proftpd
      > sudo systemctl status proftpd
      > sudo ufw reload
