# Windows FYSA Commands

## Find all token / user data:
```whoami /all```
```
C:\Users\KILLSWITCH-GUI>whoami /all

USER INFORMATION
----------------

User Name                      SID
============================== ==============================================
desktop-<SNIP> \killswitch-gui <SNIP>


GROUP INFORMATION
-----------------

Group Name                                                    Type             SID                                                                                              Attributes
============================================================= ================ ================================================================================================ ==================================================
Everyone                                                      Well-known group S-1-1-0                                                                                          Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account and member of Administrators group Well-known group S-1-5-114                                                                                        Group used for deny only
BUILTIN\Administrators                                        Alias            S-1-5-32-544                                                                                     Group used for deny only
BUILTIN\Users                                                 Alias            S-1-5-32-545                                                                                     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                                      Well-known group S-1-5-4                                                                                          Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                                                 Well-known group S-1-2-1                                                                                          Mandatory group, Enabled by default, Enabled group
<SNIP>                     


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Enabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled


```

## Get Local ipconfig data:
```ipconfig /all```
```
C:\Users\KILLSWITCH-GUI>ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-<SNIP>
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : <SNIP>-router.home

Ethernet adapter Ethernet 2:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : PANGP Virtual Ethernet Adapter
   Physical Address. . . . . . . . . : 02-50-41-00-00-01
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::344b:2314:f01d:a51%8(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.0.0.235(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.255
   Default Gateway . . . . . . . . . : 0.0.0.0
   DHCPv6 IAID . . . . . . . . . . . : 419582017
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-20-E4-0D-90-30-9C-23-04-69-34
   DNS Servers . . . . . . . . . . . : ::1
                                       127.0.0.1
   NetBIOS over Tcpip. . . . . . . . : Enabled
   <SNIP>
```

## List all virtual / physical drives with Powershell:
```gdr -PSProvider 'FileSystem```
```
PS C:\Users\KILLSWITCH-GUI> gdr -PSProvider 'FileSystem'

Name           Used (GB)     Free (GB) Provider      Root                                               CurrentLocation
----           ---------     --------- --------      ----                                               ---------------
B                   0.00          0.25 FileSystem    B:\
C                 138.56        325.85 FileSystem    C:\                                           Users\KILLSWITCH-GUI
D                4503.95       3872.36 FileSystem    D:\
<SNIP>
```

```[System.IO.DriveInfo]::GetDrives() | Format-Table```
```
Name DriveType DriveFormat IsReady AvailableFreeSpace TotalFreeSpace TotalSize     RootDirectory VolumeLabel
---- --------- ----------- ------- ------------------ -------------- ---------     ------------- -----------
A:\    Network               False                                                 A:\
C:\      Fixed NTFS          True  771920580608       771920580608   988877418496  C:\           Windows
D:\      Fixed NTFS          True  689684144128       689684144128   1990045179904 D:\           Big Drive
E:\      CDRom               False                                                 E:\
G:\    Network NTFS          True      69120000           69120000       104853504 G:\           GratefulDead
```


## Check if host is alive via cmd:
```
ping -n 1 host.com - overt
nbtstat -A host.com - Covert: uses NetBios TCP/IP to check if interface is up
```

## List the password policy for the domain:
```
net accounts 
```
```
C:\Users\KILLSWITCH-GUI>net accounts
Force user logoff how long after time expires?:       Never
Minimum password age (days):                          0
Maximum password age (days):                          42
Minimum password length:                              0
Length of password history maintained:                None
Lockout threshold:                                    Never
Lockout duration (minutes):                           30
Lockout observation window (minutes):                 30
Computer role:                                        WORKSTATION
```

## Get the current DC your talking to:
```
cmd /c echo %LOGONSERVER%
powershell echo $ENV:LOGONSERVER
```

## Resolve host-name to IP addr IPv4 with out ping via Powershell:
```
[System.Net.DNS]::GetHostAddresses("NAME-PC")
```
```
PS C:\Users\KILLSWITCH-GUI> [System.Net.DNS]::GetHostAddresses("google.com")


Address            : 2382879148
AddressFamily      : InterNetwork
ScopeId            :
IsIPv6Multicast    : False
IsIPv6LinkLocal    : False
IsIPv6SiteLocal    : False
IsIPv6Teredo       : False
IsIPv4MappedToIPv6 : False
IPAddressToString  : 172.217.7.142
```

## Resolve ip to host-name:
```powerview
[System.Net.Dns]::GetHostbyAddress("8.8.8.8")
```
```
PS C:\Users\KILLSWITCH-GUI> [System.Net.Dns]::GetHostbyAddress("8.8.8.8")

HostName                       Aliases AddressList
--------                       ------- -----------
google-public-dns-a.google.com {}      {8.8.8.8}
```

## List last boot time via Powershell:
```powershell 
gwmi Win32_OperatingSystem | select __SERVER,@{label='LastRestart';expression={$_.ConvertToDateTime($_.LastBootUpTime}}
```