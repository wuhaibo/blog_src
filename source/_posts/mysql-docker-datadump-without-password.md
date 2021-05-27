title: mysql datadump without password
author: haibo
tags:
  - mysql
categories:
  - 技術
date: 2021-05-26 10:46:00
---
> 很多時候需要執行mysql脚本，在執行脚本的時候需要指定用戶名密碼，把密碼放在脚本裏不太安全，本文介紹一> 種方法，把密碼放在配置文件中，脚本執行的時候會自動從配置文件中讀取密碼。
<!--more-->

- 第一步創建 ```~/.my.cnf```文件

```
[mysqldump]
user = mysqluser
password = secret
```

- 第二步修改權限
```bash
chmod 600 ~/.my.cnf
```
- 測試
```bash
mysqldump  -uroot  --opt $dbname$  
```






