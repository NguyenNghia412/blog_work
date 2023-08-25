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

## SysV
Trong SysV, các stage (mode) được định nghĩa khác với Boot target.

1. Red Hat based system thì có 7 stage:

    - 0- Shutdown
    - 1- Single-user mode (recovery); Also called S or s
    - 2- Multi-user without networking
    - 3- Multi-user with networking
    - 4- to be customized by the admin (empty, người dùng tự custom)
    - 5- Multi-user with networking and graphics
    - 6- Reboot

2. Debian based system có 4 stage:
    - 0- Shutdown
    - 1- Single-user mode
    - 2- Multi-user mode with graphics (khác với red hat based)
    - 6- Reboot

### Command

|runlevel|Xem stage trước và stage hiện tại là gì|
|init 0 |Shutdown máy|
|init 6|Restart máy|

## Shutdown system

Linux thường được lựa chọn làm server cho nhiều loại dịch vụ khác nhau nên sẽ có nhiều user đang cùng tty vào 1 server. Khi Sys Admin muốn off server đi thì ko thể chạy lệnh `shutdown now` ngay được, mà nên thông báo với người dùng là 10 phút nữa server sẽ tắt, thế nó mới lịch sự. Trong 10p đó, server sẽ block hết các session của các user khác muốn đăng nhập vào server.

Về các mode shutdown thì có 3 lệnh tương ứng như dưới đây. Thường trong các distro, các lệnh này thường là symbol link dẫn tới thư mục khác để execute

![](/assets/images/runlevel-boottargets/halt-sl.png)

|halt|Ngưng hoạt động system|
|poweroff|Ngưng giống `halt` và tắt máy|
|reboot|Giống `poweroff` nhưng thêm khởi động nữa|

Có thể dùng `shutdown` theo nhiều cách để halt/off/reboot system. Xem `man shutdown` để biết thêm chi tiết

|shutdown -P now|tắt luôn|
|shutdown -P|tắt máy|
|shutdown -r|restart máy|
|shutdown -H|halt|
|shutdown -r 10|10 nữa reboot|
|shutdown -r 10:55|10:55 thì reboot|
|shutdown -P 5 may tinh chuan bi tat|5p nữa tắt máy và gửi broadcast msg cho toàn bộ user đang login tty|
|**Thông báo cho user**|
|wall hello|gửi broadcast msg cho toàn bộ user đang login tty|
|mesg y|Bật chế độ các user đều nhận được tin nhắn từ `wall`. Nếu tắt đi thì thay y là **n**|
|vi /etc/motd|Nội dung trong file này sẽ được show ra mỗi khi có user login tty vào system|

Broadcast message nó trông như thế này
![](/assets/images/runlevel-boottargets/broadcast.png)

Sau khi sửa nội dung trong /etc/motd, user login tty vào thì màn hình sẽ trông như thế này
![](/assets/images/runlevel-boottargets/motd.png)