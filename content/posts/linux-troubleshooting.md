---
title: "Linux Troubleshooting"
description: "Some Linux troubleshooting"
date: 2023-02-06T10:10:09+01:00
draft: false
categories:
- Linux
tags:
- linux
---

## Freeze
You can try 
```
Ctrl+Alt+*
```
 to kill the front process (Screen locking programs on Xorg 1.11) or
```
 Ctrl+Alt+F1
```
 to open a terminal, launch a command like ps, top, or htop to see running processes and launch kill on not responding process.
Note: if not installed, install htop with 
```
sudo apt-get install htop
```
. 
Also, once done in your 
```
Ctrl+Alt+F1
```
 virtual console, return to the desktop with
```
 Ctrl+Alt+F7.
```

On laptops you might need to press Ctrl+Fn+F1 to open terminal, what I do is type reboot now to restart from terminal. 
To go back to the GUI from terminal  on my laptop (HP G56) I have to Ctrl+Fn+F8 (apparently it could also be Ctrl+Fn+F7) and you should be back to graphical interface. 
Also check http://community.linuxmint.com/tutorial/view/244
Stopping & Starting
- shutdown -h now – Shutdown the system now and do not reboot
- halt – Stop all processes - same as above
- shutdown -r 5 – Shutdown the system in 5 minutes and reboot
- shutdown -r now – Shutdown the system now and reboot
- reboot – Stop all processes and then reboot - same as above
- startx – Start the X system

## Ethernet OK, no Internet

```
sudo nano /etc/resolv.conf
add 8.8.8.8 & 8.8.44
me-user@my-machine:/etc$ sudo service network-manager restart
```

This has to do with resolvconf being installed in your system.
So the best way to keep something in resolv.conf that won't get lost on reboot is to include it in resolvconf configuration files that are in:
```
/etc/resolvconf/resolv.conf.d/
```
In there go for the head file. Whatever you put there will be written at the top of /etc/resolv.conf
So everything will go to something like this:
```
# echo nameserver 8.8.8.8 >> /etc/resolvconf/resolv.conf.d/head
# resolvconf --enable-updates
# resolvconf -u
```
