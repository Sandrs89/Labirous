## ################################################################
# 1. UFW Firewall
UFW (Uncomplicated Firewall or "simple firewall") is an iptables interface 
designed to simplify the firewall configuration process.

 ## 1.1 Step:
    > sudo apt-get install ufw
    > sudo ufw enable
    > sudo ufw status
    
 ## Lan:
    > sudo ss -tnlp
    > ss -lntp | sed -r 's/\t/ /g'
    > netstat
    > netstat -lnpvut (посмотрим на текущие соединения)
    
 ## 1.2 Control:
    > sudo ufw allow 20/tcp
    > sudo ufw deny 20/tcp
   
    > sudo ufw delete allow 20/tcp
    > sudo ufw delete deny 20/tcp
    
    > sudo ufw status numbered
    > sudo ufw delete

  ## 1.3 Open port  
    > sudo ufw allow 20/tcp
    > sudo ufw allow 21/tcp
    > sudo ufw allow 40000:65000/tcp
    > sudo ufw allow 22/tcp
    > sudo ufw allow 23/tcp 
    > sudo ufw allow 80/tcp 
    > sudo ufw allow 443/tcp
