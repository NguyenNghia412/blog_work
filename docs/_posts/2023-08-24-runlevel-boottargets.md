---
layout: post
title:  "Runlevels & boot targets"
date:   2023-08-24 21:54
categories: [Linux, Kernel, Lpic, Boot]
---
## Tổng quan
**Runlevel** (với SysV) hay **boot targets** (với Systemd) có thể được hiểu là các mode của Linux OS.

Khi boot system lên, nó sẽ được trải qua các *mode single user (root)* => *multi user* => *graphical interface (Giao diện đồ họa)* => *networking* => ... => *shutdown*. Đấy thì mỗi mode đó được gọi là một runlevel/boot targets. 

User có thể switch qua lại giữa các mode hoặc vào mỗi mode này độc lập nếu mode cho phép **Isolate**. Tức là ví dụ có thể chỉ vào riêng mode **emergency**, lúc đấy sẽ không còn network hay đồ họa nữa.

Ví dụ như hình dưới đây, unit này **cho phép isolate**:
![](/assets/images/runlevel-boottargets/graphical-target.png)

## Boot target (Systemd)
Một vài lệnh

|systemctl list-units --type=target|Show all boot targets|
|systemctl get-default|Xem unit nào là defaut target (Thường là graphical)|
|systemctl cat graphical.target|Xem cấu hình target|
|systemctl isolate rescue|Vào target nào đó với chế độ isolate|

### Minh họa cấu hình của 1 target (*graphical*)
![](/assets/images/runlevel-boottargets/minhhoa-target.png)


