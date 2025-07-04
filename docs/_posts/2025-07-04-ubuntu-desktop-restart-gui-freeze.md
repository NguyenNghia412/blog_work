---
layout: post
title:  "Ubuntu desktop restart GUI bị treo"
date:   2025-07-04
categories: [Ubuntu, Desktop, GUI, Restart]
---

- Check xem đang dùng service nào để chạy GUI

`cat /etc/X11/default-display-manager`

=> /usr/sbin/gdm3

- Restart lại GUI

`sudo systemctl restart gdm3`