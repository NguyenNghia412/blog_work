---
layout: post
title:  "Đổi IP CentOS 7"
date:   2024-07-05
categories: [UP, Static IP, CentOS 7, CentOS]
---

To configure an eth0 interface with static network settings using ifcfg files, edit or create a file with name ifcfg-eth0 in the /etc/sysconfig/network-scripts/ directory as follows:

`vi /etc/sysconfig/network-scripts/ifcfg-eth0`

Update/edit as follows for static IP configuration:
```
# static IP address on CentOS 7 or RHEL 7#
HWADDR=00:08:A2:0A:BA:B8
TYPE=Ethernet
BOOTPROTO=none
# Server IP #
IPADDR=192.168.2.203
# Subnet #
PREFIX=24
# Set default gateway IP #
GATEWAY=192.168.2.254
# Set dns servers #
DNS1=192.168.2.254
DNS2=8.8.8.8
DNS3=8.8.4.4
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
# Disable ipv6 #
IPV6INIT=no
NAME=eth0
# This is system specific and can be created using 'uuidgen eth0' command #
UUID=41171a6f-bce1-44de-8a6e-cf5e782f8bd6
DEVICE=eth0
ONBOOT=yes
```

`systemctl restart network`