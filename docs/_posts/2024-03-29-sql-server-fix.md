---
layout: post
title:  "Sql Server - Xử lý sự cố"
date:   2024-03-29
categories: [Sql Server, MSSQL, SP, Stored Procedure, SQL]
---
### Khi muốn join db từ server A sang server B

Tại server A, đăng nhập bằng tài khoản **sa**.

Add link tới server B:

`exec sp_addlinkedserver @server='IP/Servername';`

Nếu server B yêu cầu tài khoản:

`EXEC sp_addlinkedsrvlogin @rmtsrvname='IP/Servername',
@useself=false, 
@rmtuser='username', 
@rmtpassword='password';`

Chạy câu này sẽ list ra các link tới db từ server A:

`EXEC sp_helpserver`