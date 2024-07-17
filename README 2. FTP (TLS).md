## ################################################################
# 2. FTP
File Transfer Protocol, т. е. FTP – протокол передачи файлов и, как 
понятно из полного названия, предназначен для передачи файлов между 
удалёнными компьютерами через сеть. 

 # 2.1 Шаг:
    > sudo apt-get install proftpd
    > sudo systemctl enable proftpd

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
    
    > правим /etc/proftpd/proftpd.conf 
      sudo nano /etc/proftpd/proftpd.conf 

      # /etc/proftpd/proftpd.conf -- This is a basic ProFTPD configuration file.
      
      Include /etc/proftpd/modules.conf

      User usa-ftp #hostinger
      Group usa #www-data

      DefaultRoot ~
      Port 21

      UseIPv6 off
      PassivePorts 40000 65535

      ServerName "Bolgarian"
      ServerType standalone
      DeferWelcome off
      DefaultServer on
      ShowSymlinks on

      TimeoutNoTransfer 360
      TimeoutStalled 600
      TimeoutIdle 180
      TimeoutLogin 20

      MaxInstances 8
      MaxClients 8

      MaxCLientsPerUser 8
      MaxHostsPerUser 8
      MaxClientsPerHost 8 "%m current connect, new denied client"
      MaxLoginAttempts 8 "current close sign"

      Umask 022
      AllowOverwrite off
      MultilineRFC2228 on

      <IfModule mod_ident.c>
         IdentLookups off
      </IfModule>

      ListOptions "-l"

      DenyFilter \*.*/
      AllowStoreRestart on

      <Directory /home/hostinger/www/*>
       Umask 023
       AllowOverwrite on
      </Directory>

      AuthPAM off


      <IfModule mod_dynmasq.c>
      # DynMasqRefresh 28800
      </IfModule>

      # PersistentPasswd off
      # AuthOrder mod_auth_pam.c* mod_auth_unix.c

      UseReverseDNS off
      #IdentLookups off

      # UseSendFile off
      
      TransferLog /var/log/proftpd/xferlog
      SystemLog /var/log/proftpd/proftpd.log

      #UseLastlog on
      
      <IfModule mod_quotatab.c>
      QuotaEngine off
      </IfModule>

      <IfModule mod_ratio.c>
      Ratios off
      </IfModule>
      
      <IfModule mod_delay.c>
      DelayEngine on
      </IfModule>

      <IfModule mod_ctrls.c>
      ControlsEngine off
      
      ControlsMaxClients 5
      
      ControlsLog /var/log/proftpd/controls.log

      ControlsInterval 5
      ControlsSocket /var/run/proftpd/proftpd.sock
      </IfModule>
      
      <IfModule mod_ctrls_admin.c>
      AdminControlsEngine off
      </IfModule>

      #Include /etc/proftpd/ldap.conf
      #Include /etc/proftpd/sql.conf
      #
      #Include /etc/proftpd/tls.conf
      #Include /etc/proftpd/sftp.conf
      #
      #Include /etc/proftpd/dnsbl.conf
      #Include /etc/proftpd/geoip.conf
      #Include /etc/proftpd/snmp.conf
      #
      #Include /etc/proftpd/virtuals.conf
      Include /etc/proftpd/conf.d/


      > sudo systemctl start proftpd
      > sudo systemctl restart proftpd
      > sudo systemctl status proftpd
      > sudo ufw reload
