
## ################################################################
11. Cron
cron — classic daemon (computer program in class systems 
UNIX), which is used for periodic execution of tasks at
a certain time.

-------------------------------------------------------------------
  11.1 Step:
  
     > sudo apt-get install cron
     > sudo service cron reload
     > sudo service cron restart
     > sudo systemctl status cron.service
    
   View cron:
   
     > crontab -l
   
   Edit cron:
   
     > crontab -e
-------------------------------------------------------------------

	# .---------------- minute (0 - 59)
	# |  .------------- hour   (0 - 23)
	# |  |  .---------- day    (1 - 31)
	# |  |  |  .------- month  (1 - 12)
	# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
	# |  |  |  |  |
	# *  *  *  *  * user  command to be executed
	# 1  *	* * * *	root  cd    /etc/cron.hourly
	# 2  *	* * * *	root  test -x /usr/sbin/anacron || ( /etc/cron.daily )
	# 3  *	* * * *	root  test -x /usr/sbin/anacron || ( /etc/cron.weekly )
	# 4  *	* * * *	root  test -x /usr/sbin/anacron || ( /etc/cron.montly )
	# 

	# command UTF-8 not boom
	*/1 * * * * /srv/dos2unix.sh 2>&1 /srv/cron/dos2unix.dev

	# clear log unix and mail
	* * * * 1 /srv/clearlog.sh 2>&1 /srv/cron/mailbox.dev
	* * * * 2 /srv/clearMail.sh 2>&1 /srv/cron/mailbox.dev

	# restart server
	* * 2 * * /srv/check-server.sh 2>&1 /srv/cron/check-server.dev
	*/5 * * * * /srv/check-resource.sh 2>&1 /srv/cron/check-resource.dev

	# clear temp log
	* * * * 1 rm /srv/ftp/log/System-Notify-2024-11-15.msg 2>&1 /srv/cron/mailbox.dev

	# SSL
	* * * 2 * /srv/updateSSL/site.sh 2>&1 /srv/cron/ssl.dev

	# certbot
	* * * 2 * /usr/bin/certbot renew  2>&1 /srv/cron/certbot.dev

	# php
	* 20 * * * rm /srv/ftp/log/php/sessions/*
	* 23 * * * rm /var/log/journal/*

	# END
