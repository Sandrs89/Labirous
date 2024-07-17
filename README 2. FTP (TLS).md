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
      nano /etc/proftpd/proftpd.conf 




   > sudo systemctl start proftpd
   > sudo systemctl restart proftpd
   > sudo systemctl status proftpd
   > sudo ufw reload
