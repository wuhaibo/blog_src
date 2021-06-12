---
title: ubuntu cron job
description: ubuntu cron job
date: 2021-06-12 13:34:12
updated: 2021-06-12 13:34:12
tags:
  - ubuntu
categories:
  - 技術
comments: true
---

## Installing Cron
```shell
sudo apt update
sudo apt install cron
sudo systemctl enable cron
# validate cron service is running
sudo services --status-all 
```

### Add Cron Job

```shell
# 1. add/edit cron jobs using interactive manner, edit or add cron job according to following format
crontab -e 
# minute hour day_of_month month day_of_week command_to_run

# 2. add cron jobs using pure cmd,

# 2.1 prepare crontab file like above format for cronjob, say the file name is crontab
# 2.2 cp crontab /etc/cron.d/crontab  
# 2.3 chmod 0644 /etc/cron.d/crontab
# 2.4 /usr/bin/crontab /etc/cron.d/crontab
# note an example for docker could be used as refference 
# https://github.com/wuhaibo/python_cron_job_in_docker/blob/main/Dockerfile

# list cron jobs
crontab -l
# delete cron jobs
crontab -r

```

| filed            | allowed values  |
|------------------|-----------------|
| minute           | 0-59            |
| hour             | 0-23            |
| Day of the month | 1-31            |
| month            | 1-12 or JAN-DEC |
| Day of the week  | 0-6 or SUN-SAT  |

example:
- ```* * * * *``` - Run the command every minute.
- ```12 * * * *``` - Run the command 12 minutes after every hour.
- ```*/15 * * * *``` - Run the command every 15 minutes.
- ```0 4 * * 1-5``` - Run the command every Tuesday, Wednesday, and Thursday at 4:00 AM.