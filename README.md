# SSH Websocket over Nginx
<img src="./6a90875cf3b1a90126ebd4814e9b53ab-modified.webp" width="100%" height="auto" alt="SSH Websocket">
<hr>
SSH over websocket through Nginx reverse proxy

* support path and wss
* support multiport 80/443 


### Installation
```
wget https://raw.githubusercontent.com/ozipoetra/sshws-nginx/main/install-nat.sh && chmod +x install-nat.sh && ./install-nat.sh
```

### For Xray
Use this one (Z-UI)[https://github.com/ozipoetra/z-ui]

---

#### NGINX Reverse Proxy: point to http://127.0.0.1:8880/
#### Simple Configuration (aapanel ready to use)
```
#PROXY-START/wsspath/

location ^~ /wsspath/
{
    proxy_pass http://127.0.0.1:8880/;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 52w;
}

#PROXY-END/wsspath/
```

#### Full Configuration
```
server {
	listen 443 ssl http2;
	server_name example.com;

	index index.html;
	root /var/www/html;
	ssl on;
	ssl_certificate /path/to/example.cer;
	ssl_certificate_key /path/to/example.key;
	ssl_protocols TLSv1.2 TLSv1.3;
	ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
	
	# location path
	location /wsspath path {
	if ($http_upgrade != "websocket") {
		return 404;
	}
        proxy_pass http://127.0.0.1:8880;
	proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_read_timeout 52w;
    }
}
```

### Credit
* Sulaiman SL
