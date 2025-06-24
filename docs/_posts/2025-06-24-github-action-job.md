---
layout: post
title:  "Github action job"
date:   2025-06-24
categories: [Git, Github, Github action, CI/CD]
---

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