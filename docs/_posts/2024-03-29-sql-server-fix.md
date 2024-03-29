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

Trong sp hoặc query thì thêm IP đằng trước db.table:

`SELECT table1.*
FROM table1
INNER JOIN [secondserver].[database].[schema].[table] AS table2 ON
    table1.joinfield = table2.joinfield`

#### Link tham khảo
[Ấn vào đây](https://stackoverflow.com/questions/8752477/joining-tables-from-different-servers){:target="_blank"}