---
title: "Linux Commands"
description: "Some Linux commands"
date: 2023-02-06T09:57:19+01:00
draft: false
categories:
- Linux
tags:
- linux
---


## Processes

### Kill application
```
killall chrom
```

### Kill application by window
```
xkill
```

### Kill process
```
pidof pdflatex
kill -9 pid
```

## Account management

### Delete keyrings
```
rm ~/.local/share/keyrings/*
```
## Filesystem 

### Disk space
```
sudo df -h
```

### Delete underscored files
```
find . -type f -name '._*' -delete

find . -type f -name "._*" -print
```
## Permissions

### Reload bash profile
```
source ~/.bash_profile
```

### Folder allow write
```
chmod -R g+rwx templates

chmod -R 777 templates
```

### Add User to Group
```
usermod -a -G examplegroup exampleusername
```

## System Information

### Get distribution
```
 cat /etc/*-release
```

### Policy Kit Version
```
dpkg -l | grep polkit | awk '{print $3}' | uniq
```

### Notebook Model

```
sudo dmidecode | grep -A 9 "System Information"
```
## PHP

### Get Php executable
```
which php
```
