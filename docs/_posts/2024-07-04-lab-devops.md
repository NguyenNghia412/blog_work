---
layout: post
title:  "Lab devops"
date:   2024-07-04
categories: [Script, Devops]
---

```
/home/nghiant/projects/shoeshop/shoe_shopdb.sql

CREATE USER 'shoeshop'@'host' IDENTIFIED WITH authentication_plugin BY 'password';
CREATE USER 'shoeshop'@'localhost' IDENTIFIED BY '123456
GRANT PRIVILEGE ON database.table TO 'shoeshop'@'localhost';
GRANT PRIVILEGE ON database.table TO 'shoeshop'@'localhost';
GRANT ALL PRIVILEGES ON shoeshop.* TO 'shoeshop'@'localhost';

use shoeshop;
source /home/nghiant/projects/shoeshop/shoe_shopdb.sql;

mvn install -Dmaven.test.skip=true
nohup java -jar shoe-ShoppingCart-0.0.1-SNAPSHOT.jar > output.log 2>&1 &

sudo update-alternatives --config editor

user hostname=(runas-user:runas-group) command
gitlab-runner ALL=(ALL:ALL) NOPASSWD: /bin/cp*
gitlab-runner ALL=(ALL:ALL) NOPASSWD: /bin/su*
gitlab-runner ALL=(ALL:ALL) NOPASSWD: /bin/chown*
gitlab-runner ALL=(ALL:ALL) NOPASSWD: /bin/chmod 755 -R /projects/shoeshop*
gitlab-runner ALL=(ALL:ALL) NOPASSWD: /bin/ps aux*
gitlab-runner ALL=(ALL:ALL) NOPASSWD: /bin/kill -9*
```