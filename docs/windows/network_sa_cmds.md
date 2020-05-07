# Windows Network Situational Awareness Commands

## PowerView Situational Awareness

*PowerView is a PowerShell tool to gain network situational awareness on Windows domains. It contains a set of pure-PowerShell replacements for various windows "net *" commands, which utilize PowerShell AD hooks and underlying Win32 API functions to perform useful Windows domain functionality.*

*It also implements various useful metafunctions, including some custom-written user-hunting functions which will identify where on the network specific users are logged into. It can also check which machines on the domain the current user has local administrator access on. Several functions for the enumeration and abuse of domain trusts also exist. See function descriptions for appropriate usage and available options. For detailed output of underlying functionality, pass the -Verbose or -Debug flags.*

*For functions that enumerate multiple machines, pass the -Verbose flag to get a progress status as each host is enumerated. Most of the "meta" functions accept an array of hosts from the pipeline.*[^1]

!!! note
    All command can be run via PowerPick to increase your OPSEC. Reducing your
    forensic artifact impact. *Allowing the execution of Powershell functionality without the use of Powershell.exe. Primarily this project uses.NET assemblies/libraries to start execution of the Powershell scripts*[^2]

### Get computers in LDAP search base and show the DNS name and OS only

```powershell
Get-DomainComputer -searchbase "LDAP://OU=place,OU=thing,DC=domain,DC=loves,DC=com" --Properties dnshostname,operatingsystem
```

### Computers with OS matching 2008, with a OU of intrest

```powershell
Get-DomainComputer -searchbase "LDAP://OU=place,OU=thing,DC=domain,DC=loves,DC=com" -OperatingSystem *2008* 
```

### Computers in LDAP search base and pipe host names of intrest to Get-NetSession

```powershell
powerpick Get-DomainComputer -SearchBase "LDAP://OU=place,OU=thing,DC=domain,DC=loves,DC=com" | where-object {$_.dnshostname -like "*HOST-NAME*"} | Get-NetSession 
```

### Remote Desktop Users for a machine for just medium intg RDP

```powershell
Get-NetLocalGroupMember HOST-NAME -GroupName "Remote Desktop Users"
```

### Admins for a machine for just medium intg RDP

```powershell
Get-NetLocalGroupMember HOST-NAME
```

### Corelate GPOs to domain system

```powershell
Get-NetOU -GPLink "{45172B9C-749A-479A-A9C7-4F85083CD517}" | % { Get-DomainComputer -ADSPath $_.distinguishedname -Properties dnshostname}
```

### Find all computers and pipe into local admins of machines

```powershell
Get-DomainComputer -searchbase "LDAP://OU=Location Location,OU=SOME,DC=am,DC=somthing,DC=com" -Properties name FindOne | Get-NetLocalGroupMember -Method API -Properties ComputerName,GroupName,MemberName| FT -Wrap
```

### Find all computer objects / systems that have a GPO applied

```powershell
Get-DomainOU -GPLink "{A8E139C2-8A5C-455B-905F-FF509D112E8C}" | % { Get-DomainComputer -ADSPath $_.distinguishedname -Properties dnshostname}
```

### Find all accounts with admin count set / DC sync

```powershell
Get-DomainUser -admincount -Properties samaccountname
```

### Check if user has rights to DC sync with the PDC

```powershell
Get-ObjectACL "DC=testlab,DC=local" -ResolveGUIDs | ? {
    ($_.ActiveDirectoryRights -match 'GenericAll') -or ($_.ObjectAceType -match 'Replication-Get')
}
```

### Pull all email's from user object of a certain OU and output to file for download

```powershell
get-domainuser -searchbase "LDAP://OU=place,OU=thing,DC=domain,DC=loves,DC=com" -properties cn,mail,userprincipalname,extensionattribute10,msrtcsip-primaryuseraddress | out-file -encoding ASCII C:\Windows\Tasks\contacts.txt
```

[^1]: https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#powerview
[^2]: https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerPick