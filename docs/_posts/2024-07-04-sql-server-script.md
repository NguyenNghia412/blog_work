---
layout: post
title:  "Chạy sql script bằng cli"
date:   2024-07-04
categories: [Sql Script, SQL Server, Powershell, CMD]
---

## Scripts
```
# Load the SMO assembly
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Smo") | Out-Null

# Define the server name and credentials
$serverName = "xxxx"
$login = "xx"
$password = "xxxx"
$dbname = "xxxx"
$filePath = "E:\$dbname.sql"

# Create a ServerConnection object
$serverConnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection
$serverConnection.ServerInstance = $serverName
$serverConnection.LoginSecure = $false
$serverConnection.Login = $login
$serverConnection.Password = $password

# Create a Server object using the ServerConnection
$server = New-Object Microsoft.SqlServer.Management.Smo.Server $serverConnection

# Verify the connection by accessing a property
Write-Host "Connected to server: " $server.Name

# Get a list of databases
$databases = $server.Databases

# init db
$databases[$dbname].Script() | Out-File $filePath

# Generate script for all tables: schema

foreach ($tables in $databases[$dbname].Tables) 
{
    $tables.Script() + "`r GO `r " | out-File $filePath -Append
} 

# Generate script for all tables: index

foreach ($tables in $databases[$dbname].Tables) 
{
    $tables.Script() + "`r GO `r " | out-File $filePath -Append

    #Generate script for all indexes in the specified table
    foreach($index in $tables.Indexes)
    {
        $index.Script() + "`r GO `r" | out-File $filePath -Append
    }
} 

# foreach ($db in $databases) {
#     Write-Host $db.Name
# }

```