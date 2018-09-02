# Let's Encrypt  Notes #


## Resources ##

- https://blog.benroux.me/running-multiple-https-domains-from-the-same-server/



## Install ##

Ubuntu 14.04

```
sudo apt-get update
sudo apt-get install letsencrypt
```


Ubuntu 16.04

https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-debian-8


sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx


```
server {
  server_name domain.com www.domain.com;
  
  location ~ /.well-known {
    allow all;
  }
}
```

# Generate SSL Certificates
sudo certbot certonly -a webroot --webroot-path=/path/to/webroot -d domain.com -d www.domain.com


# Generate Strong Diffie-Hellman Group 
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048


sudo ls -l /etc/letsencrypt/live/your_domain_name


conf/ssl.conf
```
  # from https://cipherli.st/
  # and https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;
  ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
  ssl_ecdh_curve secp384r1;
  ssl_session_cache shared:SSL:10m;
  ssl_session_tickets off;
  ssl_stapling on;
  ssl_stapling_verify on;
  resolver 8.8.8.8 8.8.4.4 valid=300s;
  resolver_timeout 5s;
  # Disable preloading HSTS for now.  You can use the commented out header line that includes
  # the "preload" directive if you understand the implications.
  #add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
  add_header Strict-Transport-Security "max-age=63072000; includeSubdomains";
  add_header X-Frame-Options DENY;
  add_header X-Content-Type-Options nosniff;
  
  # sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
  ssl_dhparam /etc/ssl/certs/dhparam.pem;
```


sites-available/example.com.conf
```
server {
  listen 80;
  listen [::]:80;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name example.com www.example.com;
  
  # Let's Encrypt
  location ~ /.well-known {
      allow all;
  }

  ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
  ssl_trusted_certificate /etc/letsencrypt/live/example.com/cert.pem;
  include conf/ssl.conf;
  
  location / {
      # Basic
      #try_files $uri =404;

      # Rewrite to index.php
      #try_files $uri /index.php?$args;

      # Rewrite to NodeJS
      try_files $uri $uri/index.html @nodejs;
  }
  
  # NodeJS
  location @nodejs {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;
      proxy_set_header X-Forwarded-Proto $scheme;

      proxy_pass http://hostname;
      proxy_redirect off;
  }
  
}

```

Check for nginx config
```
nginx -t
```


Renew certs
```
sudo certbot renew
```



crontab entry - 2:30 am
```
30 2 * * * /usr/bin/certbot renew --noninteractive --renew-hook "/bin/systemctl reload nginx" >> /var/log/le-renew.log
30 2 * * * /usr/bin/certbot renew --noninteractive --renew-hook "/usr/sbin/nginx -s reload" >> /var/log/le-renew.log
```