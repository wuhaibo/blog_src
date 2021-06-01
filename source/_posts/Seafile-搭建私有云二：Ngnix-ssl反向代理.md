---
title: Seafile 搭建私有云二：Ngnix ssl反向代理
description: 本篇介紹如何用ngnix做一個反向代理，并且指向Seafile服務。
date: 2021-05-27 11:45:06
updated: 2021-05-27 11:45:06
tags:
  - ngnix
  - 反向代理
categories:
  - 技術
---
## 准备docker-compose.yml



```
version: '3'
services:
  nginx: 
    image: nginx:latest
    container_name: production_nginx
    volumes: # mapping ngnix config file and ssl certificates
      - ./nginx.conf:/etc/nginx/ngnix.conf
      - ./certificate.pem:/etc/ssl/live/certificate.pem
      - ./private_key.key:/etc/ssl/live/private_key.key
    ports:
      - 80:80
      - 443:443
    networks:
        - seafile-net # use the network created in first step
    external_links:
        - seafile
  
networks:
  seafile-net:
    external: true
```

## 准备ssl certificate

如上把ssl 证书的pem 和私有key保存到相应位置即可。



## 准备ngnix config

```
events {

}

http {

  error_log /etc/nginx/error_log.log warn;
  client_max_body_size 20m;
  
  server {
	listen       80;
	listen       443 ssl;
    server_name www.lb-home.de;
	
	# transfer http request to https
	if ($scheme = http) {
        return 301 https://$server_name$request_uri;
    }
	# end transfer http request to https

    ssl_protocols TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers ECDH+AESGCM:ECDH+AES256:ECDH+AES128:DHE+AES128:!ADH:!AECDH:!MD5;
    ssl_certificate /etc/ssl/live/lb-home/www.lb-home.de_ssl_certificate.pem;
    ssl_certificate_key /etc/ssl/live/lb-home/www.lb-home.de_private_key.key;


	  location / {
      proxy_pass http://seafile;
	  }	
  }
}
```