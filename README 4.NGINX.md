

## ################################################################
4. Nginx 
Nginx — one of the most popular web servers in the world, which
hosts some of the largest Internet sites with huge
traffic. This is a lightweight version that can be used as
a web server or as a reverse proxy.

-----------------------------------------------------
  4.1 Step:
  
	 > sudo apt install nginx
	 > sudo systemctl enable nginx
  
	 
	 > edit /etc/nginx/nginx.conf
	   nano /etc/nginx/nginx.conf

  
	 > sudo nginx -t
	 > sudo nginx -s reload

  
	 > sudo systemctl reload nginx
	 > sudo systemctl restart nginx
	 > sudo systemctl status nginx

-----------------------------------------------------
   firewall UFW:
   
     > sudo ufw allow 443/tcp
     > sudo ufw allow 443,8080/tcp
