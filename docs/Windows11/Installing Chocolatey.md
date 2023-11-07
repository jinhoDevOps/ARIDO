#choco


https://community.chocolatey.org/courses/installation/installing?method=install-from-powershell-v3

## Requirements

- Windows 7+ / Windows Server 2003+
- PowerShell v2+
- .NET Framework 4+ (the installation will attempt to install .NET 4.0 if you do not have it installed)
## Install from PowerShell v3+

With PowerShell, there is an additional step or two. You must ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is not Restricted. We suggest using `Bypass` to bypass the policy to get things installed or `AllSigned` for quite a bit more security.

- Run `Get-ExecutionPolicy`. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass`.
- Now run the following command:
```sh
Set-ExecutionPolicy Bypass -Scope Process -Force; iwr https://community.chocolatey.org/install.ps1 -UseBasicParsing | iex
```

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force;
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072;
iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
```