---
layout: post
title:  "Restore db mới từ file bak của db cũ"
date:   2025-06-23
categories: [SqlServer, MSSQL]
---
### Lưu ý: Không cần tạo db trước. Chạy lệnh xong MSSQL tự tạo

### Xem và copy giá trị từ cột Logical Name.
`RESTORE FILELISTONLY FROM DISK='[Đường dẫn đến file bak nằm trên server]'`

- Query sẽ trả về 2 dòng. Thường sẽ gồm [Tên db cũ] và [Tên db cũ]_log.
- Copy lại 2 cái đó để dùng cho bước sau

### Chạy lệnh này sẽ restore db mới từ bak của db cũ

- Copy lệnh sau, thay các tham số vào rồi chạy

```
RESTORE DATABASE [Tên db mới] FROM DISK='[Đường dẫn đến file bak nằm trên server]'
WITH 
   MOVE '[Tên db cũ]' TO 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\[Tên db mới].mdf',
   MOVE '[Tên db cũ]_log' TO 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\[Tên db mới]_log.mdf'
```
