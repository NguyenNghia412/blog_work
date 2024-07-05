---
layout: post
title:  "Cài đặt wazuh và lưu ý"
date:   2024-07-05
categories: [Wazuh, CentOS, Filebeat]
---

## Tài liệu tham khảo

- [Giới thiệu](https://documentation.wazuh.com/current/getting-started/architecture.html){:target="_blank"}.

- [Hướng dẫn cài step by step](https://documentation.wazuh.com/current/installation-guide/wazuh-indexer/step-by-step.html){:target="_blank"}.

- [Cũng là hướng dẫn cài nhưng ko phải từ trang chủ](https://medium.com/@akobeajiboluemmanuel/step-by-step-setup-of-wazuh-siem-on-ubuntu-22-04-3-lts-4663104fe69b){:target="_blank"}.

## Các port sử dụng

| Component       | Port           | Protocol                                       | Purpose                   |
|-----------------|----------------|------------------------------------------------|---------------------------|
| Wazuh server    | 1514           | TCP (default)                                  | Agent connection service  |
| 1514            | UDP (optional) | Agent connection service (disabled by default) |
| 1515            | TCP            | Agent enrollment service                       |
| 1516            | TCP            | Wazuh cluster daemon                           |
| 514             | UDP (default)  | Wazuh Syslog collector (disabled by default)   |
| 514             | TCP (optional) | Wazuh Syslog collector (disabled by default)   |
| 55000           | TCP            | Wazuh server RESTful API                       |
| Wazuh indexer   | 9200           | TCP                                            | Wazuh indexer RESTful API |
| 9300-9400       | TCP            | Wazuh indexer cluster communication            |
| Wazuh dashboard | 443            | TCP                                            | Wazuh web user interface  |


## Mở port cho Wazuh dashboard interface

```
firewall-cmd --permanent --zone=public --add-port=443/tcp
firewall-cmd --permanent --zone=public --add-port=1514/tcp
firewall-cmd --permanent --zone=public --add-port=1515/tcp
firewall-cmd --reload
firewall-cmd --permanent --zone=public --list-ports
```

## Tài khoản / Mật khẩu mặc định

Tài khoản này dùng cho cả **Indexer**, **Server** và **Dashboard**

**username**: admin
**password**: admin