---
title: Seafile 搭建私有云之二：ngnix ssl 反向代理
description: 在docker里实用ngnix ssl 反向代理seafile云服务
date: 2021-06-01 14:07:37
updated: 2021-06-01 14:07:37
tags:
  - ngnix，反向代理
categories:
  - 技术
comments: true
---
## 准备docker-compose.yml

```
version: '3'
services:
  nginx: 
    image: nginx:latest
    container_name: production_nginx
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./certificate.pem:/etc/ssl/live/certificate.pem
      - ./private_key.key:/etc/ssl/live/private_key.key
    ports:
      - 80:80
      - 443:443
    networks:
        - seafile-net
    external_links:
        - seafile
  
networks:
  seafile-net:
    external: true
```



## 准备ssl certificates

依照如上位置存放ssl certificates 和private key



## 准备ngnix config

```
events {

}

http {

  error_log /etc/nginx/error_log.log warn;
  client_max_body_size 20m;
  
  
  server {
	
	server_name  host.com; # process the request with host as host.com
	listen       80;
	listen       443 ssl;
	
	ssl_certificate /etc/ssl/live/lb-home/fullchain.pem; # ssl certificate
    ssl_certificate_key /etc/ssl/live/lb-home/privkey.pem; # private key
	
	location / { 
	    # common ngnix reverse proxy options
		proxy_http_version  1.1;
		proxy_cache_bypass  $http_upgrade;

		proxy_set_header Upgrade           $http_upgrade;
		proxy_set_header Connection        "upgrade";
		proxy_set_header Host              $host;
		proxy_set_header X-Real-IP         $remote_addr;
		proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Forwarded-Host  $host;
		proxy_set_header X-Forwarded-Port  $server_port;		
		# end common ngnix reverse proxy options
		
		proxy_pass http://localhost:80;  # forword request to http://localhost:80;
		index index.html; # index is index.html
		    if ($scheme = http) {
        return 301 https://$server_name$request_uri;
    }
	}	
  }
}
```

准备ngnix config 文件，并存入相应位置。