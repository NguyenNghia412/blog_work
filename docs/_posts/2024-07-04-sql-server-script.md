---
layout: post
title:  "Chạy sql script bằng cli"
date:   2024-07-04
categories: [Sql Script, SQL Server, Powershell, CMD]
---
## Sử dụng

- Powershell
- Microsoft.SqlServer.Smo
- Microsoft.SqlServer.Management.Smo.Server
- Microsoft.SqlServer.Management.Smo.Scripter
- Microsoft.SqlServer.Management.Common.ServerConnection
- Microsoft.SqlServer.Management.SMO.ScriptingOptions

## Lệnh chạy

- Thay các biến Servername, login, password, dbname, filePath, listTables

- Mở powershell. Chạy lệnh
`gen-script-mssql.ps1`

## Nội dung scripts
```
# Load the SMO assembly
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Smo") | Out-Null

# Define the server name and credentials
$serverName = "xxxxxxxxx" # IP, localhost or servername
$login = "xx" # User
$password = "xxxxxxxx" # Password
$dbname = "xxxxxxxxx" # Database
$filePath = "E:\$dbname-$((Get-Date).ToString('ddMMyyyy_HHmm')).sql" # Đường dẫn lưu file script sql
$listTables = @("Users", "Test") # List bảng muốn xuất script

# Create a ServerConnection object
$serverConnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection
$serverConnection.ServerInstance = $serverName
$serverConnection.LoginSecure = $false
$serverConnection.Login = $login
$serverConnection.Password = $password

# Create a Server object using the ServerConnection
$server = New-Object Microsoft.SqlServer.Management.Smo.Server $serverConnection

# Lấy database cần xuất script
$db = $server.Databases[$dbname]
Write-Output($db.Name)

# Database object để sinh script
$Objects = $db.Tables
$Objects += $db.Views
$Objects += $db.StoredProcedures
$Objects += $db.UserDefinedFunctions

# Options sinh script
$scriptingOptions = New-Object Microsoft.SqlServer.Management.SMO.ScriptingOptions
# $scriptingOptions.ScriptData = $True
$scriptingOptions.AppendToFile = $True
$scriptingOptions.FileName = $filePath
$scriptingOptions.AllowSystemObjects = $False
$scriptingOptions.ClusteredIndexes = $True
$scriptingOptions.DriAll = $True
$scriptingOptions.IncludeDatabaseContext = $True
$scriptingOptions.ScriptDrops = $False
$scriptingOptions.IncludeHeaders = $False
$scriptingOptions.ToFileOnly = $True
$scriptingOptions.Indexes = $True
$scriptingOptions.Permissions = $True
$scriptingOptions.WithDependencies = $False
# $scriptingOptions.Encoding = [System.Text.Encoding]::ASCII

# Export
$scripter = New-Object Microsoft.SqlServer.Management.Smo.Scripter ($server)
$scripter.Options = $scriptingOptions

# Sinh script db
# Write-Output("Generate database...")
# $db.script() | out-file $filePath

# Sinh script schema bảng, sp các thứ
# Write-Output("Generate schema...")
# $scripter.Script($Objects)

# Sinh script data
Write-Output("Generate data...")
$scriptingOptions.ScriptData = $True

# Xuất toàn bộ bảng
# $scripter.EnumScript(@($db.Tables))

# Xuất vài bảng
foreach($tbl in $listTables) {
    Write-Output("Generate table $tbl...")
    $scripter.EnumScript($db.Tables[$tbl])
}

Write-Output("DONE")



```