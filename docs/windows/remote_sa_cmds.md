# Windows Remote Situational Awareness Commands

## WMI Remote Situational Awareness

### Remote process listing of machine

```powershell
gwmi Win32_Process -ComputerName 43.*.*.5 | % {$name = $_.ProcessName; $ProcessOwner = ($_.GetOwner().User);$ProcID=$_.ProcessId;"$name`t`t$ProcessOwner`t$ProcID"}
```

## PowerPick Situational Awareness

### Remote process listing of machine via Powerpick WMI with $credential object

```powershell
powerpick $credential = New-Object System.Management.Automation.PSCredential ("DA\some",("TestPassword" | ConvertTo-SecureString -AsPlainText -Force)); gwmi Win32_Process -ComputerName <name> -Credential $credential | ?{ $_.ProcessId -match "PID" }.Terminate()
```

### Remote last boot time listing with PowerPick and $credential object

```powershell
powerpick $credential = New-Object System.Management.Automation.PSCredential ("DA\some",("TestPassword" | ConvertTo-SecureString -AsPlainText -Force));
gwmi Win32_OperatingSystem -ComputerName <name> -Credential $credential | select __SERVER,@{label='LastRestart';expression={$_.ConvertToDateTime($_.LastBootUpTime}}
```