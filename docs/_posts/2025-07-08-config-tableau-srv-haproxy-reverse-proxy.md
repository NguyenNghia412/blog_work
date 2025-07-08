---
layout: post
title:  "Cấu hình Ha proxy làm reverse proxy cho Tableau Server"
date:   2025-07-04
categories: [Tableau Server, Ha Proxy]
---

## Bối cảnh

- Cấu hình Ha Proxy làm reverse proxy cho Tableau Server

- Mô hình Client => Ha Proxy => Tableau Server

- Cấu hình proxy bình thường trên HA đến Tableau ko được nên mới có bài này


## Tài liệu

- [Bài viết](https://help.tableau.com/current/server/en-us/proxy.htm#configure-tableau-server-to-work-with-a-reverse-proxy-server){:target="_blank"}

- Đọc phần **Configure Tableau Server to work with a reverse proxy server and/or load balancer** để cấu hình trên Tableau Server

- Đọc phần **Configure the reverse proxy or load balancing server to work with Tableau Server** để cấu hình trên Reverse Proxy (HA Proxy)

## Thực hành

### Cấu hình trên Ha Proxy

- Cấu hình như này cho **Backend Server** dẫn đến **Tableau** (Cái này cho doc vào gpt nó tự phòi ra)

```
backend tableau_srv
    # Real client IP chain
    http-request set-header X-Forwarded-For %[src]
    http-request set-header REMOTE_ADDR %[src]

    # Original host
    http-request set-header Host %[req.hdr(host)]
    http-request set-header X-Forwarded-Host %[req.hdr(host)]

    # Protocol
    http-request set-header X-Forwarded-Proto https if { ssl_fc }
    server tableau_srv_1 192.168.0.15:80 check

```

Xong thì restart haproxy

`systemctl restart haproxy`

- Cấu hình trên **Tableau Server**

```
tsm configuration set -k gateway.public.host -v "[Tên miền]"
tsm configuration set -k gateway.trusted_hosts -v [Tên miền]
tsm configuration set -k gateway.trusted -v "[IP của Haproxy server]"
tsm configuration set -k gateway.public.port -v 443 => Nếu có ssl thì dùng cái này
tsm pending-changes apply
```

- Chờ Tableau service khởi động lại là test được