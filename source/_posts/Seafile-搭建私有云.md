---
title: Seafile 搭建私有云一：搭建Seafile
author: haibo
description: 記錄一下自己搭建私有云的過程.
date: 2021-05-27 11:43:49
updated: 2021-05-27 11:43:49
tags:
  - Seafile
categories:
  - 技術
---
> 記錄一下自己搭建私有云的過程。这里主要介绍seafile的部分。
>
> 过程分两个博文
>
> [Seafile 搭建私有云一：搭建Seafile](https://friendly-mcnulty-50c07e.netlify.app/2021/05/27/seafile-%E6%90%AD%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BA%91/)
>
> [Seafile 搭建私有云之二：ngnix ssl 反向代理](https://friendly-mcnulty-50c07e.netlify.app/2021/06/01/seafile-%E6%90%AD%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BA%91%E4%B9%8B%E4%BA%8C%EF%BC%9Angnix-ssl-%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86/)



## 目標

本文假設要把seafile用以docker的形式安裝在ubuntu 18上. 
然後使用ngnix作爲反向代理實現https。 

## 安裝Seafile

### 安裝docker-compose

```bash
sudo apt-get install docker-compose -y
```

### 準備docker-compose.yml文件

```yml
version: '2.0'
services:
  db:
    image: mariadb:10.5
    container_name: seafile-mysql
    environment:
      - MYSQL_ROOT_PASSWORD=  # Requested, set the root's password of MySQL service.
      - MYSQL_LOG_CONSOLE=true
    volumes:
      - /opt/seafile-mysql/db:/var/lib/mysql  # Requested, specifies the path to MySQL data persistent store.
    networks:
      - seafile-net

  memcached:
    image: memcached:1.5.6
    container_name: seafile-memcached
    entrypoint: memcached -m 256
    networks:
      - seafile-net
          
  seafile:
    image: seafileltd/seafile-mc:latest
    container_name: seafile
    expose:
      - "80"
#      - "443:443"  # If https is enabled, cancel the comment.
    volumes:
      - /opt/seafile-data:/shared   # Requested, specifies the path to Seafile data persistent store.
    environment:
      - DB_HOST=db
      - DB_ROOT_PASSWD=  # Requested, the value shuold be root's password of MySQL service.
#      - TIME_ZONE=Asia/Shanghai # Optional, default is UTC. Should be uncomment and set to your local time zone.
      - SEAFILE_ADMIN_EMAIL= # Specifies Seafile admin user, default is 'me@example.com'.
      - SEAFILE_ADMIN_PASSWORD= # Specifies Seafile admin password, default is 'asecret'.
      - SEAFILE_SERVER_LETSENCRYPT=false   # Whether use letsencrypt to generate cert.
      - SEAFILE_SERVER_HOSTNAME= # Specifies your host name.
    depends_on:
      - db
      - memcached
    networks:
      - seafile-net

networks:
  seafile-net:
```

這裏要把
`MYSQL_ROOT_PASSWORD`， `DB_ROOT_PASSWD`，`SEAFILE_ADMIN_EMAIL`，`SEAFILE_ADMIN_PASSWORD`，`SEAFILE_SERVER_HOSTNAME`
設定為自己需要的值。

另外

```yml
networks:
  seafile-net:
```

這裏表明這個文件的services共用一個叫 `seafile-net`的網絡。

docker Seafile service 于主機共享了  `/opt/seafile-data（host）:/shared（container）`

### 啓動Seafile server

```yml
networks:
  seafile-net:
```

### Log Path

* `/opt/seafile-data/logs/seafile` 是seafile的日志
* `/opt/seafile-data/logs/seafile` 是container的日志

### 備份和恢復

目標：建立一個ubuntu cron job 來定時備份

#### 建立備份脚本

```bash
cronjob_name="backupSeafile"
today=$(date +%Y%m%d_%I%M%S)
log_file="/home/williamwood/cronjob/cronjob.log"

echo "\n\n*******************run $cronjob_name at $today******************" >> $log_file

#generate backup data locally
data_folder=/opt/seafile-backup/data

## 增量備份
rsync -az /opt/seafile-data/seafile /opt/seafile-backup/data/
cd /opt/seafile-backup/data && rm -rf ccnet

## 簡單copy 備份
# cp -R /opt/seafile-data/seafile /opt/seafile-backup/data/
# cd /opt/seafile-backup/data && rm -rf ccnet

echo "seafile data backup done" >> $log_file

docker exec -it seafile-mysql mysqldump  -uroot -p$password$ --opt ccnet_db > /opt/seafile-backup/databases/ccnet_db.sql
docker exec -it seafile-mysql mysqldump  -uroot -p$password$ --opt seafile_db > /opt/seafile-backup/databases/seafile_db.sql
docker exec -it seafile-mysql mysqldump  -uroot -p$password$ --opt seahub_db > /opt/seafile-backup/databases/seahub_db.sql
echo "seafile database backup done" >> $log_file

echo "**********************************end***********************************" >> $log_file
```

#### 創建cronjob

* 以root賬戶執行cronjob脚本

```bash
sudo cronjob -e
```

* 脚本執行后寫入任務執行時間和執行脚本

<img src="https://res.cloudinary.com/dr8wkuoot/image/upload/v1621939199/blog/cronjob_fvbqfw.jpg">

### 恢復

#### 恢復database

```bash
docker cp /opt/seafile-backup/databases/ccnet_db.sql seafile-mysql:/tmp/ccnet_db.sql
docker cp /opt/seafile-backup/databases/seafile_db.sql seafile-mysql:/tmp/seafile_db.sql
docker cp /opt/seafile-backup/databases/seahub_db.sql seafile-mysql:/tmp/seahub_db.sql

docker exec -it seafile-mysql /bin/sh -c "mysql -uroot -p$psd$ ccnet_db < /tmp/ccnet_db.sql"
docker exec -it seafile-mysql /bin/sh -c "mysql -uroot -p$psd$ seafile_db < /tmp/seafile_db.sql"
docker exec -it seafile-mysql /bin/sh -c "mysql -uroot -p$psd$ seahub_db < /tmp/seahub_db.sql"
```

#### 恢復Seafile data

```bash
cp -R /opt/seafile-backup/data/* /opt/seafile-data/seafile/
```

## 碰到的問題

* 問題：修改網站地址后（比如測試時用了ip作爲網址，後來改為域名），資源網址沒改，導致錯誤avatars圖片錯誤。
* 解決辦法，重啓memcached 

```bash
docker restart seafile-memcached
```