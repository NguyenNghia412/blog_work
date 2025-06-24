---
layout: post
title:  "Một vài lệnh tương tác vcenter"
date:   2025-06-24
categories: [VCenter, ESXI, VSphere]
---

## Kết nối vcenter, bật tắt VM

```
 Find-Module -Name VMware.PowerCLI
Install-Module -Name VMware.PowerCLI
 Get-PowerCLIVersion
Install-Module -Name VMware.PowerCLI -Scope CurrentUser

Connect-VIServer [IP VCENTER SERVER]
Connect-VIServer [domain trỏ vào vcenter]

# Graceful Shutdown (Preferred if VMware Tools is installed)
Stop-VM -VM (Get-VM -Name "Lab.Moodle_192.168.100.102") -Confirm:$false

# Force Power Off
Stop-VM -VM (Get-VM -Name "web01") -Confirm:$false -Kill
Lab.DEV
Stop-VM -VM (Get-VM -Name "Lab.Nghia-SOHOA_192.168.100.32") -Confirm:$false -Kill



Get-VM | Where-Object { $_.PowerState -eq "PoweredOn" } | Select-Object Name, PowerState, @{Name="IP";Expression={$_.Guest.IPAddress}}

Get-VM | Where-Object { $_.PowerState -eq "PoweredOn" } | Select-Object Name
```