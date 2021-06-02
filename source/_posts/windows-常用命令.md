---
title: windows 常用命令
description: 本文记录一些windows常用的命令
date: 2021-06-02 09:56:57
updated: 2021-06-02 09:56:58
tags:
  - windows，cmd
categories:
  - 技术
comments: false
---
### get wifi password

```powershell
netsh wlan show profile name=$wlanName$ key=clear
```

### Get windows Service with execute path

```powershell
Get-WmiObject win32_service  | select Name, state, PathName | Sort-Object state | Export-Csv "service.csv"
```

### ftp cmd

```powershell
ftp $destination$ 
## give user name and password
## ? for help
## ! escape the cmd to shell
## send file use put
```

### Get scheduled job/tasks

```powershell
schtasks /query /v /fo LIST

## powershell, this is not working for windows before windows 2012
get-scheduledtask | get-scheduledtaskinfo | select TaskName, TaskPath
```

### find file

```
dir /s *foo*.txt
```



## IIS

### Tool to View the IIS Log : Logparser

<https://blogs.msdn.microsoft.com/friis/2014/02/06/how-to-analyse-iis-logs-using-logparser-logparser-studio/>

### Use Cmd to Export/Import IIS Websites

```powershell
# To Export the Application Pools
%windir%\system32\inetsrv\appcmd list apppool /config /xml > c:\apppools.xml

# To import  the Application Pools
%windir%\system32\inetsrv\appcmd add apppool /in < c:\apppools.xml

# To Export all your website:
%windir%\system32\inetsrv\appcmd list site /config /xml > c:\sites.xml

# To Import all your website:
%windir%\system32\inetsrv\appcmd add site /in < c:\sites.xml

# To export/import a single application pool:
##export
%windir%\system32\inetsrv\appcmd list apppool “MyAppPool” /config /xml > c:\myapppool.xml
##import
%windir%\system32\inetsrv\appcmd add apppool /in < c:\myapppool.xml

# To export/import a single website:
##export
%windir%\system32\inetsrv\appcmd list site “MyWebsite” /config /xml > c:\mywebsite.xml
##import
%windir%\system32\inetsrv\appcmd add site /in < c:\mywebsite.xml
```