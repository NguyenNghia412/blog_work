---
layout: post
title:  "PostgreSQL Command"
date:   2023-08-26 10:04
categories: [PostgreSQL]
---

Trên windows vào psql shell. *Cài xong postgresql trên windows, ấn tổ hợp Windows + S, search psql là thấy*
Lưu ý là sau mỗi lệnh thì phải có ; ở cuối câu thì psql mới exec.

*Trông lệnh có vẻ cũng giống mysql.*

## Tạo user

|`create user nghiant with password '564654';`|Tạo user với password|
|`alter user nghiant with password '564654';`|Cập nhật password cho user|
|`\du;`|Show user xem đã hiện chưa|

## Tạo database

|`create database demo;`|Tạo database;|
|`grant all privileges on database testdb to nghiant;`|Trao quyền thao tác database cho user|
|`ALTER DATABASE mydb OWNER TO admin;`|Trao role owner đối với database này cho user khác. (Lúc bị lỗi permission denied for schema public, chạy nhiều lệnh nhưng cuối cùng lệnh này fix được) |
|`GRANT ALL ON SCHEMA public TO ussername;`||
|`GRANT CREATE ON SCHEMA schema_name TO username;`||
|`GRANT USAGE ON SCHEMA public TO username;`||