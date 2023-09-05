---
layout: post
title:  "Dùng powershell thao tác với Google Admin"
date:   2023-08-31 11:29
categories: [Powershell, Google Admin, PSGSuite, GAM]
---

## Cài đặt PSGSuite

- Tài liệu tham khảo:
    - [https://aventistech.com/2020/07/22/manage-google-g-suite-with-powershell/](https://aventistech.com/2020/07/22/manage-google-g-suite-with-powershell/){:target="_blank"}
    - [https://psgsuite.io/Initial%20Setup/](https://psgsuite.io/Initial%20Setup/){:target="_blank"}

## Sử dụng PSGsuite
Mở powershell với quyền **admin**.

(*Nếu đang ở trong File Explorer, có thể ấn Alt + F => S => A*)

|`Get-GSUserList | Select User`|Xem tất cả user|
|`Get-GSUser dat46862@example.com`|Tìm user theo email|
|`New-GSUser "demo68@example.com" -GivenName A -FamilyName "Nguyen Van" -FullName "Nguyen Van A" -Password (ConvertTo-SecureString  "xxxxx" -AsPlainText -Force) -OrgUnitPath "/XXXX XX/YYY" -Addresses @((add-gsuseraddress -type home -formatted "Thanh Xuân Hà Nội"), (add-gsuseraddress -type work -formatted "CNTT")) -organizations (add-gsuserorganization -department "68XD1" -title "Sinh viên") -Phones ( Add-GSUserPhone -Type home -Value "03xxxxxxx7") -Emails (Add-GSUserEmail -Type home -Address test@gmail.com) -ExternalIds (Add-GSUserExternalId -Type organization -Value 002665887552) -ChangePasswordAtNextLogin:$true`|Khởi tạo user|
|`Update-GSUser -User demo68@example.com -RecoveryEmail recovery@gmail.com -RecoveryPhone +849xxxxxxx7 -Confirm:$false`|Cập nhật **số điện thoại khôi phục** và **email khôi phục**. Sdt phải có định dạng +84|
|`Remove-GSUser dat46862@example.com -Confirm:$false`|Xoá user|
|`Restore-GSUser -User dat46862@example.com -Confirm:$false`|Xoá user|
|`Update-GSUser [-User]  [-PrimaryEmail ] [-FullName ] [-Password ] [-ChangePasswordAtNextLogin] [-OrgUnitPath ] [-Suspended] [-Addresses ] [-Emails ] [-ExternalIds ] [-Ims ] [-Locations ] [-Organizations ] [-Relations ] [-RecoveryEmail ] [-RecoveryPhone ] [-Phones ] [-IncludeInGlobalAddressList] [-IpWhitelisted] [-IsAdmin] [-Archived] [-CustomSchemas ] [-WhatIf] [-Confirm] []`||
|`$extId = Add-GSUserExternalId -Type organization -Value 002665887552`|Tạo object cmnd|
|`Import-Csv -Path "E:\Doc\Huce\K68\K68-email-test.csv" | foreach {New-GSUser $_.User -GivenName $_.Firstname -FamilyName $_.Lastname -FullName $_.Fullname -Password (ConvertTo-SecureString $_.Password -AsPlainText -Force) -OrgUnitPath $_.Org -Addresses @((add-gsuseraddress -type home -formatted $_.Address), (add-gsuseraddress -type work -formatted $_.Khoa)) -organizations (add-gsuserorganization -department $_.Department -title "Sinh viên") -Phones ( Add-GSUserPhone -Type home -Value $_.Phone) -Emails (Add-GSUserEmail -Type home -Address $_.Email) -ExternalIds (Add-GSUserExternalId -Type organization -Value $_.ExternalID) -ChangePasswordAtNextLogin:$true} | Export-Csv -Path "E:\Doc\Huce\K68\logs\K68-email-test.log"`|Tạo **user** từ file csv|
|`Import-Csv -Path "E:\Doc\Huce\K68\K68-email-test.csv" | foreach {Update-GSUser -User $_.User -RecoveryEmail $_.Email -RecoveryPhone $_.RecoveryPhone -Confirm:$false} | Export-Csv -Path "E:\Doc\Huce\K68\logs\K68-email-test.log"`| Update **recovery email và phone** từ csv|
|||
|||
|||
|||
|||

## Sử dụng GAM
