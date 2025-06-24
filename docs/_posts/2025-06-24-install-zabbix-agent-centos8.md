---
layout: post
title:  "Cài zabbix agent trên CentOS8"
date:   2025-06-24
categories: [Linux, Centos8]
---

## Hướng dẫn cài đặt zabbix agent trên centos 8

```
rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/8/x86_64/zabbix-release-5.0-1.el8.noarch.rpm
dnf clean all

dnf install zabbix-agent
systemctl enable --now zabbix-agent

vi /etc/zabbix/zabbix_agentd.conf

Server=[ip zabbix server]
ServerActive=[ip zabbix server]
Hostname=[pc name]

systemctl restart zabbix-agent

firewall-cmd --add-service={http,https} --permanent
firewall-cmd --add-port={10051/tcp,10050/tcp} --permanent
firewall-cmd --reload

➜  ~ openssl rand -hex 32
8093cec0570c12b304c47cf9c83c5d9a4de4d7

vi /etc/zabbix/zabbix_agentd.psk

TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=dhxd
TLSPSKFile=/etc/zabbix/zabbix_agentd.psk
```
