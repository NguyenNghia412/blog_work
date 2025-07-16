---
layout: post
title:  "Dùng MS Graph tương tác tài khoản Azure Microsoft Office"
date:   2025-07-16
categories: [Ms Graph, Powershell, Cli, Azure, Windows, Microsoft Office]
---

## Nguyên nhân

- Sử dụng MsolService như cách cũ bên dưới không được nữa vì Microsoft bỏ => Chuyển sang dùng MS Graph Module

```
$msolcred = Get-Credential
Connect-MsolService -Credential $msolcred
```

- Nếu vẫn dùng cách cũ thì sẽ gặp lỗi

```
Connect-MsolService : Access Denied. You do not have permissions to call this cmdlet.
At line:1 char:1
+ Connect-MsolService -Credential $msolcred
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : OperationStopped: (:) [Connect-MsolService], MicrosoftOnlineException
    + FullyQualifiedErrorId : Microsoft.Online.Administration.Automation.AccessDeniedException,Microsoft.Online.Admini
   stration.Automation.ConnectMsolService

Connect-MsolService : Exception of type 'Microsoft.Online.Administration.Automation.MicrosoftOnlineException' was
thrown.
At line:1 char:1
+ Connect-MsolService -Credential $msolcred
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : OperationStopped: (:) [Connect-MsolService], MicrosoftOnlineException
    + FullyQualifiedErrorId : Microsoft.Online.Administration.Automation.MicrosoftOnlineException,Microsoft.Online.Adm
   inistration.Automation.ConnectMsolService
```

## Cài đặt và sử dụng

### Cài đặt

- Mở powershell quyền admin. Cài module

`Install-Module Microsoft.Graph -Scope CurrentUser`

### Một vài trường hợp sử dụng

- Get thông tin user

```
Connect-Graph -Scopes User.ReadWrite.All
Get-MgUser -userid 'nghiant@outlook.com'
```

- Reset mật khẩu user (Không bắt buộc đổi password sau lần đầu đăng nhập)

```
Connect-Graph -Scopes User.ReadWrite.All, User-PasswordProfile.ReadWrite.All
Update-MgUser -UserId '0123456@outlook.com' -PasswordProfile @{ Password = 'newpasswordA@'; ForceChangePasswordNextSignIn = $false }
```