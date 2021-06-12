---
title: docker 常用命令
description: docker 常用命令
date: 2021-06-12 13:34:12
updated: 2021-06-12 13:34:12
tags:
  - docker
categories:
  - 技術
comments: true
---

### image相關
```shell
# 編譯image
sudo docker build -t $imagename$ $Path_to_Dockerfile$

# 列出本机的所有 image 文件。
sudo docker image ls

# 删除 image 文件
sudo docker image rm $imageName$

```

### container相關

```shell
# 得到正在運行的container
sudo docker container ls

# 列出本机所有容器，包括终止运行的容器
sudo docker container ls --all

# start container
sudo docker container start $containerID$

# remove container
sudo docker container -rm $containerName$

# 停止container
sudo docker container stop $containerId$

# 得到運行中container的shell
sudo docker exec -it $containerName or containerId$ /bin/bash

# docker container logs
sudo docker container logs [containerID]

# docker container cp
sudo docker container cp [containID]:[/path/to/file] 

```

### dockerfile 相關
一個例子：https://github.com/wuhaibo/python_cron_job_in_docker



```dockerfile
FROM python:3.7

RUN apt-get update && apt-get -y install cron vim

WORKDIR /app

# setup cron job
COPY crontab /etc/cron.d/crontab
RUN chmod 0644 /etc/cron.d/crontab
RUN /usr/bin/crontab /etc/cron.d/crontab

# setup for python app
COPY . /app
RUN pip3 install -r /app/src/requirements.txt

# run
CMD ["cron", "-f"]

```
- FROM python:3.7 以python image為基礎製作image
- RUN apt-get update && apt-get -y install cron vim。 RUN 可以表示在image中用來執行shell命令
- WORKDIR /app 指明並創建工作文件夾是/app
- COPY crontab /etc/cron.d/crontab 把本機（host）的文件（./crontab）copy到image的/etc/cron.d/crontab
- CMD ["cron", "-f"] docker文件中必須有至少CMD 或者ENTRYPOINT命令。 
  * exec 模式和 shell 模式:
  ```dockerfile
  CMD ["executable","param1","param2"] // 这是 exec 模式的写法，注意需要使用双引号。

  CMD command param1 param2 // 这是 shell 模式的写法。
  ``` 
  exec 模式是建议的使用模式, exec 模式的特点是不会通过 shell 执行相关的命令, 不過可以這樣寫
  ```CMD [ "sh", "-c", "echo $HOME" ]```。使用 shell 模式时，docker 会以 /bin/sh -c "task command" 的方式执行任务命令。也就是说容器中的 1 号进程不是任务进程而是 bash 进程

### Note
- 在DockerFile中写入的CMD后面的命令不执行主要是因为启动的时候指定了shell