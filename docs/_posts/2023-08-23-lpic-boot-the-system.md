---
layout: post
title:  "Boot the system"
date:   2023-08-23 23:08
categories: [Linux, Kernel, Lpic]
---

## Quá trình boot
1. Khi khởi động, firmware trên mainboard sẽ thực hiện test các phần cứng (RAM, ổ cứng, cpu,...) xem oke không. Khi firmware thực hiện kiểm tra thì được gọi là **PowerOnSelfTest**.
2. Tiếp theo mainboard sẽ load tới **bootloader** (`GRUB`, `GRUB 2`).
3. **Bootloader** sẽ load **Kernel** dựa trên config của nó.
4. Kernel sẽ load (cùng với driver cần thiết như `initramfs`) và bắt đầu thực hiện init các process.
5. Start các service cần thiết (web server, graphical interface, networking,...)

## Firmware
Firmware là **phần mềm** nằm ngay trong **phần cứng** và có chức năng start hệ thống phần cứng đó. Có thể xem firmware như os hoặc driver của phần cứng.

Firmware trên mainboard máy tính trước đây là BIOS, sau này là UEFI.

1. BIOS *(basic input output system)*
    - BIOS thường được dùng làm firmware trong các máy tính ngày trước.
    - BIOS rất nhỏ. Chỉ chứa trong sector đầu tiên của ổ cứng (512 byte). Phần đó trên ổ cứng gọi là MBR (Master boot record).
    - Khi khởi động thì không thể trực tiếp load kernel luôn mà phải qua multi-stage bootloader (thường là 2 stage: thằng đầu là basic vì ko làm đc gì nhiều trong 512 byte, thằng đó sẽ start tiếp thằng 2. Thằng này thì làm được nhiều thứ hơn).
    - Có thể start từ ổ HDD, CD/DVD, USB,...
2. UEFI *(unified extensible firmware interface)*
    - Hiện đại hơn BIOS
    - Phân vùng chứa UEFI có thể lớn bao nhiêu cũng được. Phân vùng này có định dạng FAT, được gọi là ESP (EFI System Partition). ESP trong linux được mount vào `/boot/efi` và file có định dạng là .efi.

## Bootloader
- Bootloader sẽ init *rất ít* phần cứng cần thiết để boot system. Sau đó sẽ khởi động OS.
- **GRUB** là Bootloader thường được sử dụng trên các Linux distro.
- Trên lý thuyết, người ta có thể chỉ cho UEFI khởi động bất kỳ thứ gì nhưng thường sẽ là GRUB. Và người ta cũng có thể cấu hình cho GRUB khởi động bất kỳ thứ gì nhưng thường nó sẽ khởi động OS.

## Kernel
Kernel thì ai cũng biết nó là core của OS. 
Thực ra nó chính là Linux. Còn các distro như Ubuntu, CentOS, RHEL thì gọi là GNU Linux nhưng gọi là Linux cho nhanh.

- Trong khi máy tính khởi động, bootloader sẽ load kernel vào ram và chạy. Tuy nhiên, một mình kernel không tự chạy được mà cần có một vài tham số để chạy. Các tham số được để trong `initramfs`.
- Khi kernel khởi động, vì đang khởi động nên kernel ko có quyền ghi file log. Nên nó lưu log vào *data ring buffer* (memory của kernel).
- Để xem log thì có vài lệnh phía dưới

    |dmesg|
    |dmesg \| less|
    |less /var/log/boot.log|
    |less /var/log/boot.log|
    |journalctl -k|

## Init
Kernel khởi động xong thì sẽ khởi động các program khác. Kernel sử dụng Init System để làm các công việc như vậy

Có các loại Init System khác nhau:
- SysVinit
- Systemd
- Upstart 

|which init|xem thằng Init System nằm ở đâu|
|pstree|Show thứ tự start các process|

### Systemd
Systemd bao gồm 12 unit khác nhau. Mỗi unit bao gồm một tập các service nào đó. Có 2 unit quan trọng nhất là *target* và *service*.

Sử dụng command `systemctl` để thao tác với các unit và `journalctl` để xem log.

```
systemctl list-units
systemctl list-units --type=target
systemctl get-default # default target (groups of services are started via target unit files)
```

Các unit thường nằm ở

|/etc/systemd/system|
|/run/systemd/system|
|/usr/lib/systemd/system|

Một vài lệnh thao tác với `unit` bằng `systemctl`:

|systemctl list-unit-files|List các unit file|
|systemctl cat ntpd.service|Xem nội dung file cấu hình của unit|
|systemctl start sshd|Start unit|
|systemctl stop sshd|Stop unit|
|systemctl status sshd|Xem unit còn chạy hay không|
|systemctl enable sshd|Để khi start máy lần nữa, thì unit cũng sẽ được khởi động cùng|
|systemctl reload sshd|Systemd đọc lại file cấu hình của chính unit|
|systemctl daemon-reload sshd|Systemd đọc lại file cấu hình của systemd dành cho unit đó|

Một vài lệnh xem log bằng `journalctl`:

|journalctl -xe|Xem log gần đây|
|journalctl -u sshd|Xem log của riêng unit này|
|journalctl -PID=1234|Xem theo id process|
|journalctl -S -1d|Xem log 1 ngày vừa qua|