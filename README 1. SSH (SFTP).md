## ################################################################
# 1. SSH (SFTP)
SSH or Secure Shell - This is a protocol for secure access from one
computer to another over a network.

 ## 1.1 Step:
      > sudo apt install openssh-server
      > sudo systemctl enable sshd
      > ssh username@ip -p port
   ---------------------------------------
   
      > edit /etc/ssh/sshd_config
        sudo nano /etc/ssh/sshd_config
	
        	Include /etc/ssh/sshd_config.d/*.conf
	
	      	ListenAddress 91.215.152.114
	      	Port 22
	
	      	LoginGraceTime 30
	      	MaxStartups 5
	      	MaxAuthTries 2

	      	LogLevel ERROR
	      	PubkeyAuthentication yes
	      	PermitRootLogin no

	      	PasswordAuthentication yes
	      	PermitEmptyPasswords no
	      	ChallengeResponseAuthentication no

	      	UsePAM yes
	      	UseDNS yes
	      	PrintMotd no

		AcceptEnv LANG LC_*
		ClientAliveInterval 20
		ClientAliveCountMax 3

		Compression yes
		Subsystem sftp internal-sftp
	 	Match group blg
	 	X11Forwarding no

	 	ChrootDirectory /
	 	#ForceCommand internal-sftp
   ----------------------------------------     

   	> edit /etc/ssh/ssh_config
     	  sudo nano /etc/ssh/ssh_config
	
 	  Include /etc/ssh/ssh_config.d/*.conf
	  Host *
	    HashKnownHosts yes
	    ServerAliveInterval 60
	    ServerAliveCountMax 5

	  Compression yes
   ----------------------------------------

   	> edit /etc/motd
     	  sudo nano /etc/motd
		===========================
		-- Welcome to Bolgarian Server --
		===========================

  	> sudo sshd -t
  	> sudo systemctl restart sshd
 	> sudo systemctl status sshd
