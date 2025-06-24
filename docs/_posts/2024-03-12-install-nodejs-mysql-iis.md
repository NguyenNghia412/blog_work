---
layout: post
title:  "Deploy app nodejs + express + mysql lên IIS (Windows Server 2016)"
date:   2024-03-12
categories: [Nodejs, Mysql, Express, IIS, Windows server, Xampp]
---
### Cài Url Rewrite
[Ấn vào đây để tải](https://www.iis.net/downloads/microsoft/url-rewrite){:target="_blank"}

### Cài iisnode
[Ấn vào đây để tải](https://github.com/tjanczuk/iisnode){:target="_blank"}

### Add file web.config vào folder của project
```
<configuration>
  <system.webServer>

    <!-- indicates that the hello.js file is a node.js application 
    to be handled by the iisnode module -->

    <handlers>
      <add name="iisnode" path="app.js" verb="*" modules="iisnode" />
    </handlers>

    <rewrite>
      <rules>
        <rule name="myapp">
          <match url="/*" />
          <action type="Rewrite" url="app.js" />
        </rule>
      </rules>
    </rewrite>
    
  </system.webServer>
    <system.web>
        <identity impersonate="false" />
    </system.web>
</configuration>
```

### Mở cmd với quyền administrator và chạy lệnh sau
`net stop was /y & net start w3svc`

### Cài xampp
[Link cài](https://www.apachefriends.org/download.html){:target="_blank"}

Lưu ý: Khi cài, cần module nào thì tích vào module đó thôi, bỏ tích module khác. Nếu chỉ cần MySQL thì chọn mỗi MySQL thôi, bỏ tích tomcat linh tinh,... Apache không bỏ chọn được thì tắt service apache đi cho đỡ conflict với iis.

### Mysql
#### Backup db cũ ra file my_db.sql
`mysqldump -u root my_db > c:\my_db.sql`

#### Restore db cũ sang server mới
`create databse my_db`

`use my_db`

`source c:\my_db.sql`