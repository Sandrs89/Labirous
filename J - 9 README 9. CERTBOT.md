

## ################################################################
9. Certbot
What is Certbot? This is an ACME protocol client designed for 
automatic management of SSL certificates from Let's Encrypt, it
allows you to fully automate the process of obtaining and renewing
a certificate, and when using the appropriate plugins, it can even 
automatically configure a web server or other
application that uses a certificate.

-------------------------------------------------------------------

  9.1 Step:
  
    > sudo apt-get install software-properties-common
    > sudo add-apt-repository ppa:certbot/certbot          ( if errors!)
    > sudo apt-add-repository -r ppa:certbot/certbot       (analog)
    > sudo apt update
    > sudo apt-get update
    > sudo apt install python-certbot-nginx                ( if errors!)
    > sudo apt-get install certbot python-certbot-apache   (analog)
    
    > sudo apt install certbot python3-certbot-nginx       (analog)
    > sudo apt-get install certbot python3-certbot-apache  (analog)
    
    
    # for Lets` encrypt
    > sudo certbot certonly --agree-tos --email gromov.sandrs@gmail.com 
    --webroot -w /home/hostinger/bin-tmp -d labirous.com -d www.labirous.com
    
    # for acme.ssl.com
    sudo certbot --nginx --agree-tos --email gromov.sandrs@gmail.com 
    --no-eff-email --config-dir /home/hostinger/bin-tmp --eab-kid b94a14e1d78d 
    --eab-hmac-key XhqV7g5wFyKxYrn9MpNOrFTTvAAonv6JoTkpBr_PiOg 
    --server https://acme.ssl.com/sslcom-dv-rsa -d labirous.com -d www.labirous.com
    
    connect module apache
    > a2enconf certbot
    > sudo service apache2 reload
    
  Let’s Encrypt 90 day.
  
    > sudo systemctl status certbot.timer  
    > sudo certbot renew --dry-run --cert-name labirous.com --authenticator webroot --webroot-path /home/hostinger/bin-tmp

  ---------------------------------------------
  > certbot certificates

    > sudo ls -lR /etc/letsencrypt/archive/     

  ---------------------------------------------
  update ssl:
  
    > sudo certbot renew --dry-run
