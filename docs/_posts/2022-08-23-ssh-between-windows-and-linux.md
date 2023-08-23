---
layout: post
title:  "Cấu hình kết nối ssh giữa windows và linux passwordless"
date:   2023-08-23
categories: [Linux, Centos, Jenkins, Gitlab]
---

Steps to establish passwordless SSH between Linux ⬌ Windows:
Note:
- Open a PowerShell console with Administrator privileges and execute all the commands mentioned below in that console only
- Depending on install path, add `C:\Windows\System32\OpenSSH` or `C:\Program Files\OpenSSH` to the System Path

---
# Windows Server 2019:
- Ensure the system is up to date via Windows Update
- Ensure OpenSSH features are installed:
    - Apps & Features > Manage Optional Features
    - OpenSSH Server and OpenSSH Client should be listed, if they are not: Add a Feature

---
# Windows Server 2012 and 2016:
1. Download [OpenSSH (OpenSSH-Win64.zip)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)
2. Extract the contents to `C:\Program Files\OpenSSH` and enter directory
3. Follow steps 4 - 6 mentioned in the [Install Wiki](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH):

```
# In an elevated Powershell console, run the following:
  powershell -ExecutionPolicy Bypass -File install-sshd.ps1

# Open the firewall for sshd.exe to allow inbound SSH connections
  New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22

# Start sshd (this will automatically generate host keys under %programdata%\ssh if they don't already exist)
  net start sshd ; net start ssh-agent
```

---
# Common Steps for Windows Server 2012/2016/2019:
1. Execute the following, which should show the status as Running for both services:

    |Set-Service ssh-agent -StartupType Automatic|
    |Set-Service sshd -StartupType Automatic|
    |Get-Service -Name ssh-agent,sshd|

    If not running: open *Services* and start `OpenSSH Server` and `OpenSSH Authentication Agent`

2. For public-private key pair generation, issue `ssh-keygen` and follow the prompts

3. Create `C:\ProgramData\ssh\administrators_authorized_keys`

    |New-Item -ItemType file "C:\ProgramData\ssh\administrators_authorized_keys"|
    
4. Append `/root/.ssh/id_rsa.pub` to `C:\ProgramData\ssh\administrators_authorized_keys`
    - If id_rsa.pub does not exist on Linux, generate via: ssh-keygen

5. Append `C:\Users\Administrator\.ssh\id_rsa.pub` to `/root/.ssh/authorized_keys`
    - If authorized_keys does not exist:

    |touch "/root/.ssh/authorized_keys"|

6. For permission settings: (Chạy 2 lệnh icacls xong, thì add lại System và Administrator vào file `administrators_authorized_keys`)

    |icacls "C:\ProgramData\ssh\administrators_authorized_keys" /remove "NT AUTHORITY\Authenticated Users"|
    |icacls "C:\ProgramData\ssh\administrators_authorized_keys" /inheritance:r|
    |Restart-Service -Name sshd, ssh-agent -Force|

---
### Relevant locations on Windows host:

`C:\Windows\Sytem32\OpenSSH\`

`C:\Program Files\OpenSSH\`

`C:\Users\Administrator\.ssh\`

`C:\ProgramData\ssh\`

---
### References:

- Microsoft Docs: [OpenSSH Key Management](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement)
- Win32-OpenSSH Github: [Install Wiki](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)
- Win32-OpenSSH Github: [Logging Facilities](https://github.com/PowerShell/Win32-OpenSSH/wiki/Logging-Facilities)

### Nguồn tham khảo:

- [https://superuser.com/questions/1319402/passwordless-ssh-from-linux-to-windows](https://superuser.com/questions/1319402/passwordless-ssh-from-linux-to-windows)
- [https://stackoverflow.com/questions/58651797/unable-to-get-public-key-login-to-work-on-windows-10-ssh-server](https://stackoverflow.com/questions/58651797/unable-to-get-public-key-login-to-work-on-windows-10-ssh-server)
