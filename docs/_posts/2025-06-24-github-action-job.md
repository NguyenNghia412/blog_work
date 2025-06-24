---
layout: post
title:  "Github action job"
date:   2025-06-24
categories: [Git, Github, Github action, CI/CD]
---

## Cài github runner

### Error
```
[doc5s@app code]$ docker-compose -v

Docker Compose version v2.32.1

[doc5s@app code]$ sudo docker-compose up -d

sudo: docker-compose: command not found

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
### Nếu server public ssh
Github action: Github nghe sự kiện trên branch của repo, ssh vào server xong build

### Nếu server private ko public ssh

```
./config.sh --url https://github.com/NguyenNghia412/5sdb --token ARO677DQPQJFKNDTHWMGJCH --work /home/doc5s/doc5s.dev

[doc5s@app actions-runner]$ sudo ./svc.sh install
Creating launch runner in /etc/systemd/system/actions.runner.NguyenNghia412-5sdb.doc5s.dev.service

sudo ./svc.sh install
sudo systemctl start actions.runner.NguyenNghia412-5sdb.doc5s.dev.service
sudo systemctl enable actions.runner.NguyenNghia412-5sdb.doc5s.dev.service
sudo systemctl status actions.runner.NguyenNghia412-5sdb.doc5s.dev.service
sudo systemctl stop actions.runner.NguyenNghia412-5sdb.doc5s.dev.service


./svc.sh stop
./svc.sh uninstall


git remote set-url origin git@github.com:YOUR-USERNAME/YOUR-REPO.git
```
## File cấu hình github job

```
name: Deploy doc5s Staging
on:
  push:
    branches:
      - dev

jobs:
  deploy:
    runs-on: [self-hosted, linux, x64]
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
          clean: false  # Prevents deletion of untracked files

      - name: Deploy code
        run: |
          cd /home/doc5s/doc5s.dev/5sdb/5sdb/
          git pull origin dev
          sudo docker-compose up -d --build
```