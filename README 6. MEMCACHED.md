
## ################################################################
7. Memcached
   
        Memcached – It is a convenient high-performance in-memory data storage. 
        A thoughtful, scalable, open source solution provides a response time of 
        fractions of a millisecond, which allows use it as a cache or session storage.

  7.1 Step:
  
    > sudo apt install memcached libmemcached-tools
    > sudo apt-cache policy memcached
    
    > sudo apt install php-memcached apache2 libapache2-mod-php php php-cli php-memcached php-memcached
    > phpenmod memcached && sudo service apache2 restart 
    
    > sudo apt install memcached php-memcached php-memcache
    
    > sudo systemctl enable memcached
    > sudo systemctl start memcached
    > sudo systemctl status memcached
    
    > sudo groupadd memcache
    > adduser root memcache 
    > usermod -aG sudo memcache  

  -----------------------------------------
 	 > edit /etc/memcached.conf
	   nano /etc/memcached.conf
	   
-----------------------------------------
logfile /home/hostinger/log/memcached.log

		# -v                                 # Be verbose
		# -vv                                # more verbose
		
		-m 64
		-p 11211                             # port is 11211
		-u memcache
		-l 127.0.0.1                         # IP addresses
		
		# -c 1024                            # Limit  1024 
		# -k                                 # paged memory.
		# -M                                 # Return error 
		# -r                                 # Maximize core
		
		-P /var/run/memcached/memcached.pid  # Use a pidfile

  -----------------------------------------
    > sudo mkdir /srv/ftp/log/memcached/
     && sudo chmod 0777 /srv/ftp/log/memcached

  -----------------------------------------
    > sudo service memcached restart
    > sudo service memcached status
    > ss -antpl | grep memcached
    > php -m | grep memcache
    
-----------------------------------------
    
  firewall UFW:
  
    > sudo ufw allow 11211/tcp
