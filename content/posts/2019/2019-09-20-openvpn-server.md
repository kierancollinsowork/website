---
title: How to Setup a VPN Server and Clients Using OpenVPN
author: Chris Titus
type: post
date: 2019-09-21T04:45:08+00:00
url: /openvpn-server/
image: /wp-content/uploads/2019/09/openvpn.png
categories:
  - Linux
  - Networking
tags:
  - OpenVPN

---
In this article, I go over how to setup a VPN Server and clients using OpenVPN. this will cover the setup process of the remote machine and then connecting to it via both Linux and Windows client machines. <!--more-->

## OpenVPN Server Setup

This is the Installation script I use to setup a secure OpenVPN Server  
<https://github.com/angristan/openvpn-install>

Run the following script as root or add sudo to the install.sh script  
`curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh`  
 `chmod +x openvpn-install.sh`  
`AUTO_INSTALL=y ./openvpn-install.sh`

you will need to enable the OpenVPN service to auto-start so the VPN stays up after reboot.  
`sudo systemctl enable openvpn`

### Troubleshooting:

![tun-isnot-available](/wp-content/uploads/2019/09/tun-isnot-available.png) 

**TUN is not available**  
-Certain VPS servers do not have TUN enabled by default. Create the follow script and run it on startup to fix this issue.

-Create /usr/sbin/enabletun.sh  
`#!/bin/bash`   
`mkdir /dev/net`   
`mknod /dev/net/tun c 10 200`   
`chmod 0666 /dev/net/tun`

-Mark /usr/sbin/enabletun.sh executable  
`chmod +x /usr/sbin/enabletun.sh`

-Run this script on startup by adding the following to /etc/rc.local  
`/usr/sbin/tunscript.sh || exit 1`  
`exit 0`

## Client Setup

### Linux Client Setup

Install OpenVPN for Network Manager  
**Debian-Based** `sudo apt install network-manager-openvpn`  
**Arch-Based** `sudo pacman -S networkmanager-openvpn`  
**Gnome-Based DEs** `sudo apt install network-manager-openvpn-gnome`

Copy OVPN file to /etc/openvpn/client/client.ovpn  
Test client configuration in Terminal:  
`sudo openvpn /etc/openvpn/client/client.ovpn`

![import-vpn](/wp-content/uploads/2019/09/import-vpn.png) 

Network manager Import VPN Connection: client.ovpn  
Note: _Certificates stored in ~/.local/share/networkmanagement/certificates_

Connect via your Network Manager

### Troubleshooting:

![tls-issue](/wp-content/uploads/2019/09/tls-issue.png) 

Verify TLS key file is enabled and created. This is a known issue on KDE desktops. If it isn&#8217;t make sure to create it using the last TLS portion of the ovpn file.

### Windows Client Setup

Download OpenVPN client for your Windows @ https://openvpn.net/community-downloads/

![clientovpn-winscp](/wp-content/uploads/2019/09/clientovpn-winscp.png?fit=1024%2C451&ssl=1) 

Copy your client.ovpn from the server (WinSCP to connect and copy) and place the file in C:\Program Files\OpenVPN\config

![openvpnconnect](/wp-content/uploads/2019/09/openvpnconnect.jpg) 

Run the program and right click the icon in the tray and connect

## Video Walkthrough

[![openvpn-yt](https://img.youtube.com/vi/CBJMl9MILbg/0.jpg)](https://www.youtube.com/watch?v=CBJMl9MILbg)  
_Note: YouTube Video - Hold Ctrl + Left Click to open in new window_

## Contact Me

I live stream on [Chris Titus Tech YouTube Channel][1] every Friday at 10 AM CST and archive clips to [Titus Tech Talk][2]. I also regularly publish to [christitus.com][3], but if you'd like to contact me directly or want to contribute to help keep these articles and videos being made consider joining the CTT members. 

Two Memberships exist:
- [ChrisTitus.com Members Section][4] (_CC Only_)
  - Full Archive of All Unlisted Live Streams
  - Direct Members Only Email
  - Monthly Members Only Video
  - Starting at $2 Per Month (_100% of Proceeds goes to Chris Titus Tech_)
- [YouTube Chris Titus Tech Membership][5] (_All Payments Accepted_)
  - YouTube Emojis for Comments and Live Chat
  - YouTube Badges that changes based on membership time for comments and chat.
  - All YouTube comments are highlighted when I review comments daily. 
  - Immediate Access to Full Live Streams
  - $4.99 Per Month (_70% of the Proceeds goes to Chris Titus Tech_)

 [1]: https://www.youtube.com/c/ChrisTitusTech
 [2]: https://www.youtube.com/c/ChrisTitusTechStreams
 [3]: https://christitus.com/
 [4]: https://christitus.com/members
 [5]: https://links.christitus.com/join