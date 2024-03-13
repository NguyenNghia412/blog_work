---
layout: post
title:  "Fix bug khi execute sp ở database mới được restored"
date:   2024-03-13
categories: [Sql Server, MSSQL, SP, Stored Procedure, SQL]
---
### Bệnh
Khi execute sp nào đó của database gặp lỗi sau (thường gặp ở database được restore từ file bak): 
```
An error occurred in the Microsoft .NET Framework while trying to load assembly id 65536. The server may be running out of resource

: 'Execution of user code in the .NET Framework is disabled. Enable "clr enabled" configuration option
```

### Cách sửa

```
USE <DATABASE>;
EXEC sp_configure 'clr enabled' ,1
GO

RECONFIGURE
GO
EXEC sp_configure 'clr enabled'   -- make sure it took
GO

USE <DATABASE>
GO

--EXEC sp_changedbowner 'sa'
--USE <DATABASE>
--GO

ALTER DATABASE <DATABASE> SET TRUSTWORTHY ON;  
```