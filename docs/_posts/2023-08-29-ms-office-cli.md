---
layout: post
title:  "Dùng powershell thao tác với MS Office Admin"
date:   2023-08-29 09:57
categories: [Powershell, MS, MS Office, MS Office Admin]
---

Mở powershell với quyền **admin**.

(*Nếu đang ở trong File Explorer, có thể ấn Alt + F => S => A*)

|`Install-Module -Name MSOnline`|Cài **Azure AD Module** cho Powershell|
|`Install-Module -Name AzureAD`|Cài **Azure AD Powershell** cho Graph module. Gõ lệnh xong chọn Y hoặc A để confirm cài tiếp.|
|`$credentials=get-credential`|Chạy lệnh xong powershell sẽ nảy ra prompt đòi nhập username/password. Sử dụng **tài khoản admin** để đăng nhập. Nhập xong thì sẽ lưu object credentials vào biến $credentials|
|`Connect-MsolService -Credential $credentials`|Kết nối với **MsOnline**. Nếu báo lỗi sai pass hoặc username thì chạy lại lệnh phía trên.|
|`Get-MsolCompanyInformation`|Check thông tin sau khi kết nối. Nếu thành công thì cli sẽ hiện list thông tin tài khoản.|
|`Get-MsolAccountSku`|**Show list license office** available trong tổ chức. *(Tên của license khi show sẽ khác trên web. [Ấn vào đây để xem danh sách các license của Office 365](https://learn.microsoft.com/en-us/MicrosoftTeams/sku-reference-edu){:target="_blank"})*|
|**Một vài license của Office 365 (SkuPartNumber)**|
|STANDARDWOFFPACK_STUDENT|Office 365 A1 for Students|
|STANDARDWOFFPACK_FACULTY|Office 365 A1 for Faculty	|
|**STANDARDWOFFPACK_IW_STUDENT**|Office 365 A1 **Plus** for Students|
|**STANDARDWOFFPACK_IW_FACULTY**|Office 365 A1 **Plus** for Faculty|
|`New-MsolLicenseOptions -AccountSkuId Contoso:BPOS_STANDARD -DisabledPlans EXCHANGE_STANDARD`|Tạo object License Options. [Command license options](https://learn.microsoft.com/en-us/powershell/module/msonline/new-msollicenseoptions?view=azureadps-1.0){:target="_blank"}|
|`New-MsolUser -FirstName "Nam" -LastName "Nguyễn Hoàng" -UserPrincipalName "namnh1@example.com" -DisplayName "Nguyễn Hoàng Nam" -Password "abcxyz@aA" -Title "Sếp" -City "Hanoi" -State "Thanh Xuan" -PostalCode "10000" -Country "Vietnam" -UsageLocation 'VN' -LicenseAssignment "myTenent:STANDARDWOFFPACK"`|**Tạo user account**. [Chi tiết](https://learn.microsoft.com/en-us/powershell/module/msonline/new-msoluser?view=azureadps-1.0){:target="_blank"}|
|`connect-azuread -Credential $credentials`|Connect với **azure ad service**|
|`Get-AzureADSubscribedSku`|**Show list office license**. [Chi tiết](https://learn.microsoft.com/en-us/microsoft-365/enterprise/view-account-license-and-service-details-with-microsoft-365-powershell?view=o365-worldwide){:target="_blank"}|
|`Install-Module Microsoft.Graph -Scope CurrentUser`|Cài **ms graph** module cho powershell|
|`Connect-Graph -Scopes "User.ReadWrite.All"`|Connect **MsGraph** trên powershell|
|`Connect-Graph -Scopes "User.Read","Application.Read.All"`||
|`Connect-Graph -Scopes User.ReadWrite.All, Organization.Read.All`||
|`Get-MgUser -userid 'nghiant@huce.edu.vn'`|Get user by username account bằng **MsGrpah**|
|`Get-MgSubscribedSku -All | Where SkuPartNumber -eq 'STANDARDWOFFPACK_IW_STUDENT' | select skuid`|Get skuid của **license**|
|`Set-MgUserLicense -UserId 'demo68@example.com' -AddLicenses @{SkuId = 'xxx-xxxx-xxxxxxxxx'} -RemoveLicenses @()`|Set **license**. [Chi tiết](https://learn.microsoft.com/en-us/powershell/module/microsoft.graph.users.actions/set-mguserlicense?view=graph-powershell-1.0){:target="_blank"}|
|`New-MgUser -GivenName "Test k68" -Surname "Nguyen Van" -UserPrincipalName "demo168@example.com" -MailNickname "demo168" -DisplayName "Nguyen Van Test k68" -PasswordProfile @{ Password="xxxxx" } -AccountEnabled -JobTitle "Sinh viên" -City "Hanoi" -State "Hai Ba Trung" -PostalCode "10000" -Country "Vietnam" -UsageLocation "VN"`|Tạo ms office user bằng ms graph|
|||
|||
|||
|||
|||
|||
