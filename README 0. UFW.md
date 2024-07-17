## ################################################################
# 0. UFW
UFW (Uncomplicated Firewall или «простой брандмауэр») представляет 
собой интерфейс iptables, предназначенный для упрощения процесса 
настройки брандмауэра.

 ## 0.1 Шаг:
    > sudo apt-get install ufw
    > sudo ufw enable
    > sudo ufw status
    
 ## Сеть:
    > sudo ss -tnlp
    > ss -lntp | sed -r 's/\t/ /g'
    > netstat
    > netstat -lnpvut (посмотрим на текущие соединения)
    
 ## Прочие команды: 
   >> sudo ufw allow 20/tcp
   >> sudo ufw deny 20/tcp
   
   >> sudo ufw delete allow 20/tcp
   >> sudo ufw delete deny 20/tcp

  ## Открываем рабочие порты  
   > sudo ufw allow 20/tcp
   > sudo ufw allow 21/tcp
   > sudo ufw allow 40000:65000/tcp
   > sudo ufw allow 22/tcp
   > sudo ufw allow 23/tcp 
   > sudo ufw allow 80/tcp 
   > sudo ufw allow 443/tcp








