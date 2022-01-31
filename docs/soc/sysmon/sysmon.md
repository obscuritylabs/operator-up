# Sysmon 


## Sysmon Cheatsheet 

### EventID 1 Process Create

The process creation event provides extended information about a newly created process. The full command line provides context on the process execution. The ProcessGUID field is a unique value for this process across a domain to make event correlation easier. The hash is a full hash of the file with the algorithms in the HashType field.

#### Event Log Entry

| Field             | Detail                                                                                                                                        |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| UtcTime           | Time in UTC when event was created                                                                                                            |
| ProcessGuid       | Process Guid of the process that got spawned/created (child)                                                                                  |
| ProcessId         | Process ID used by the OS to identify the created process (child)                                                                             |
| Image             | File path of the process being spawned/created. Considered also the child or source process                                                   |
| FileVersion       | Version of the image associated with the main process (child)                                                                                 |
| Description       | Description of the image associated with the main process (child)                                                                             |
| Product           | Product name the image associated with the main process (child) belongs to                                                                    |
| OriginalFileName  | OriginalFileName from the PE header, added on compilation                                                                                     |
| Company           | Company name the image associated with the main process (child) belongs to                                                                    |
| CommandLine       | Arguments which were passed to the executable associated with the main process                                                                |
| CurrentDirectory  | The path without the name of the image associated with the process                                                                            |
| User              | Name of the account that created the process (child) . It usually contains domain name and username                                           |
| LogonGuid         | Logon GUID of the user who created the new process. Value that can help you correlate this event with others that contain the same Logon GUID |
| LogonId           | Login ID of the user who created the new process. Value that can help you correlate this event with others that contain the same Logon ID     |
| TerminalSessionId | ID of the session the user belongs to                                                                                                         |
| IntegrityLevel    | Integrity label assigned to a process                                                                                                         |
| Hashes            | Full hash of the file with the algorithms in the HashType field                                                                               |
| ParentProcessGuid | ProcessGUID of the process that spawned/created the main process (child)                                                                      |
| ParentProcessId   | Process ID of the process that spawned/created the main process (child)                                                                       |
| ParentImage       | File path that spawned/created the main process                                                                                               |
| ParentCommandLine | Arguments which were passed to the executable associated with the parent process                                                              |
| ParentUser        | Name of the account that created the parent process. It usually contains domain name and username                                             |

#### Elastic ECS Mapping
| ECS Event Mapping | Field Data (Example) | Sysmon Field Mapping |
|----|----|----|
| event.action | N/A |
| event.category | process | |
| event.code | 1 | |
| event.created | Jan 30, 2022 @ 21:51:17.092 | UtcTime |
| event.kind | event | N/A |
| event.module | sysmon | N/A |
| event.provider | Microsoft-Windows-Sysmon | N/A |
| event.type | start, process_start | n/a |
| hash.imphash | b71cb3ac5c352bec857c940cbc95f0f3 | Hashes |
| hash.md5 | 60ff40cfd7fb8fe41ee4fe9ae5fe1c51 | Hashes |
| hash.sha1 | 3ea7cc066317ac45f963c2227c4c7c50aa16eb7c | Hashes |
| hash.sha256 | 2198a7b58bccb758036b969ddae6cc2ece07565e2659a7c541a313a0492231a3 | Hashes |
| process.args | C:\Windows\system32\rundll32.exe, C:\Windows\system32\PcaSvc.dll,PcaPatchSdbTask | CommandLine |
| process.command_line | C:\Windows\system32\rundll32.exe C:\Windows\system32\PcaSvc.dll,PcaPatchSdbTask | CommandLine |
| process.entity_id | {a754cc8d-0794-61f8-d001-000000000d00} | ProcessGuid |
| process.executable | C:\Windows\System32\rundll32.exe | Image |
| process.parent.args | C:\Windows\system32\svchost.exe, -k, netsvcs, -p, -s, Schedule | ParentCommandLine |
| process.parent.command_line | C:\Windows\system32\svchost.exe -k netsvcs -p -s Schedule | ParentCommandLine |
| process.parent.entity_id | {a754cc8d-d9d1-61f7-2600-000000000d00} | ParentProcessGuid
| process.parent.name | svchost.exe | ParentImage |
| process.parent.pid | 1632 | ParentProcessId |
| process.pe.company | Microsoft Corporation | Company |
| process.pe.description | Windows host process (Rundll32) | Description |
| process.pe.product | Microsoft® Windows® Operating System | Product |
| process.pid | 5316 | ProcessId |
| process.working_directory | C:\Windows\system32\ | CurrentDirectory |
| related.hash | dd399ae46303343f9f0da189aee11c67bd868222, ef3179d498793bf4234f708d3be28633, b53f3c0cd32d7f20849850768da6431e5f876b7bfa61db0aa0700b02873393fa, 4db27267734d1576d75c991dc70f68ac | Hashes |
| related.user | SYSTEM | User |
| user.domain | NT AUTHORITY | User |
| user.id | S-1-5-18 | LogonId |
| user.name | SYSTEM | User |


### EventID 2 File creation time changed

The change file creation time event is registered when a file creation time is explicitly modified by a process. This event helps tracking the real creation time of a file. Attackers may change the file creation time of a backdoor to make it look like it was installed with the operating system. Note that many processes legitimately change the creation time of a file; it does not necessarily indicate malicious activity.

#### Event Log Entry

| Field                   | Detail                                                                                  |
| ----------------------- | --------------------------------------------------------------------------------------- |
| UtcTime                 | Time in UTC when event was created                                                      |
| ProcessGuid             | Process Guid of the process that changed the file creation time                         |
| ProcessId               | Process ID used by the OS to identify the process changing the file creation time       |
| Image File              | path of the process that changed the file creation time                                 |
| TargetFilename          | Full path name of the file                                                              |
| CreationUtcTime         | New creation time of the file                                                           |
| PreviousCreationUtcTime | Previous creation time of the file                                                      |
| User                    | Name of the account that created the file. It usually contains domain name and username |

#### Elastic ECS Mapping
| ECS Event Mapping | Field Data (Example) | Sysmon Field Mapping |
|----|----|----|

### EventID 3 Network connection

The network connection event logs TCP/UDP connections on the machine. It is disabled by default. Each connection is linked to a process through the ProcessId and ProcessGUID fields. The event also contains the source and destination host names IP addresses, port numbers and IPv6 status.

#### Event Log Entry

| Field               | Detail                                                                             |
| ------------------- | ---------------------------------------------------------------------------------- |
| UtcTime             | Time in UTC when event was created                                                 |
| ProcessGuid         | Process Guid of the process that made the network connection                       |
| ProcessId           | Process ID used by the OS to identify the process that made the network connection |
| Image               | File path of the process that made the network connection                          |
| User                | Name of the account who made the network connection                                |
| Protocol            | Protocol being used for the network connection                                     |
| Initiated           | Indicates whether the process initiated the TCP connection                         |
| SourceIsIpv6        | Is the source IP an Ipv6 address                                                   |
| SourceIp            | Source IP address that made the network connection                                 |
| SourceHostname      | DNS name of the host that made the network connection                              |
| SourcePort          | Source port number                                                                 |
| SourcePortName      | Name of the source port being used                                                 |
| DestinationIsIpv6   | Is the destination IP an Ipv6 address                                              |
| DestinationIp       | IP address destination                                                             |
| DestinationHostname | DNS name of the host that is contacted                                             |
| DestinationPort     | Destination port number                                                            |
| DestinationPortName | Name of the destination port                                                       |

#### Elastic ECS Mapping

Example Event Log:

```text
Network connection detected:
RuleName: technique_id=T1021,technique_name=Remote Services
UtcTime: 2022-01-31 19:41:19.612
ProcessGuid: {ffc6f37f-da30-61f7-1500-000000000a00}
ProcessId: 956
Image: C:\Windows\System32\svchost.exe
User: NT AUTHORITY\NETWORK SERVICE
Protocol: tcp
Initiated: false
SourceIsIpv6: false
SourceIp: 94.232.42.95
SourceHostname: -
SourcePort: 52191
SourcePortName: -
DestinationIsIpv6: false
DestinationIp: 10.40.2.103
DestinationHostname: -
DestinationPort: 3389
DestinationPortName: -
```

| ECS Event Mapping | Field Data (Example) | Sysmon Field Mapping |
|----|----|----|
| event.code | 3 | N/A |
| event.kind | event | N/A |
| event.module | sysmon | N/A |
| event.category | Network | N/A |
| event.type | connection, start, protocol | N/A | 
| event.provider | Microsoft-Windows-Sysmon | N/A |
| event.action | Network connection detected (rule: NetworkConnect) | N/A |
| destination.domain | - | DestinationHostname |
| destination.ip | 10.40.2.103 | DestinationIp |
| destination.port | 3389 | DestinationPort |
| event.created | Jan 31, 2022 @ 14:41:22.352 | UtcTime | 
| network.community_id | 1:pFiw4iD296r81i3sN/GWjIMRpVk= | N/A |
| network.direction | ingress | Initiated |
| network.protocol | - | N/A |
| network.transport | tcp | Protocol |
| network.type | ipv4 | N/A |
| process.entity_id | {ffc6f37f-da30-61f7-1500-000000000a00} | ProcessGuid |
| process.executable | C:\Windows\System32\svchost.exe | Image |
| process.name | svchost.exe | Image |
| process.pid | 956 | ProcessId |
| related.ip | 94.232.42.95, 10.40.2.103 | N/A |
| related.user | NETWORK SERVICE | User |
| source.domain | - | SourceHostname |
| source.ip | 94.232.42.95 | SourceIp |
| source.port | 52191 | SourcePort |
| user.domain | NT AUTHORITY | User |
| user.id | S-1-5-18 | User |
| user.name | NETWORK SERVICE | User |

