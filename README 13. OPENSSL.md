
## ################################################################
14. OpenSSL
The OpenSSL utility is a tool that encrypts data for receiving and transmitting. 

-------------------------------------------------------------------

	> openssl ciphers -v | awk '{print $2}' | sort | uniq
	> openssl ciphers -v | column -t
	> sudo openssl s_client -connect setiwik.ru:443 -tls1_3
	
