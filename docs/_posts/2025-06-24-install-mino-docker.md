---
layout: post
title:  "Install minio (docker)"
date:   2025-06-24
categories: [Linux, Ubuntu, Docker, Minio, S3]
---

## Môi trường

- OS: Ubuntu 24.04.2 LTS

- Có cài docker + docker-compose

## File compose

```
version: '3.8'
services:
  minio:
    image: minio/minio
    container_name: 'minio'
    ports:
      - '9000:9000'
      - '9001:9001'
    environment:
      MINIO_ROOT_USER: [USERNAME]
      MINIO_ROOT_PASSWORD: [PASSWORD]
    volumes:
      - '/minio/data:/data'
    command: 'server /data --console-address ":9001"'

```

- [USERNAME] và [PASSWORD] là tài khoản để đăng nhập giao diện web gui của minio

- Persist data từ trong container (/data) ra ngoài (/minio/data)