# Windows Priv Esc Commands 
1. ##### Look for schduled tasks we can alter by path? they run at system:
```schtasks /query /fo LIST /v```
```
C:\Users\KILLSWITCH-GUI>schtasks /query /fo LIST /v

Folder: \
HostName:                             DESKTOP-<SNIP>
TaskName:                             \ASC10_PerformanceMonitor
Next Run Time:                        N/A
Status:                               Ready
Logon Mode:                           Interactive only
Last Run Time:                        11/30/1999 12:00:00 AM
Last Result:                          267011
Author:                               KILLSWITCH-GUI
Task To Run:                          C:\Program Files (x86)\IObit\Advanced SystemCare\Monitor.exe /Task
Start In:                             N/A
Comment:                              N/A
Scheduled Task State:                 Enabled
Idle Time:                            Disabled
Power Management:
Run As User:                          KILLSWITCH-GUI
Delete Task If Not Rescheduled:       Disabled
Stop Task If Runs X Hours and X Mins: Disabled
Schedule:                             Scheduling data is not available in this format.
Schedule Type:                        At logon time
Start Time:                           N/A
Start Date:                           N/A
End Date:                             N/A
Days:                                 N/A
Months:                               N/A
Repeat: Every:                        N/A
Repeat: Until: Time:                  N/A
Repeat: Until: Duration:              N/A
Repeat: Stop If Still Running:        N/A
<snip>
```


2. ##### Look for vuln services:
 ```net start```
```
C:\Users\KILLSWITCH-GUI>net start
These Windows services are started:

   Advanced SystemCare Service 10
   Application Information
   Application Management
   Background Intelligent Transfer Service
   Background Tasks Infrastructure Service
   Base Filtering Engine
   cFosSpeed System Service
   CNG Key Isolation
   COM+ Event System
   Computer Browser
   Connected Devices Platform Service
   Connected Devices Platform User Service_7e8e2a
   Connected User Experiences and Telemetry
   Contact Data_7e8e2a
   CoreMessaging
   <SNIP>
   ```

3. ##### Look for vuln drivers loaded, we offten dont spend enough time looking at this:
```DRIVERQUERY /FO table```
```
C:\Users\KILLSWITCH-GUI>DRIVERQUERY /FO table

Module Name  Display Name           Driver Type   Link Date
============ ====================== ============= ======================
1394ohci     1394 OHCI Compliant Ho Kernel        12/10/2006 4:44:38 PM
3ware        3ware                  Kernel        5/18/2015 6:28:03 PM
ACPI         Microsoft ACPI Driver  Kernel        12/9/1975 6:17:08 AM
AcpiDev      ACPI Devices driver    Kernel        12/7/1993 6:22:19 AM
acpiex       Microsoft ACPIEx Drive Kernel        3/1/2087 8:53:50 AM
acpipagr     ACPI Processor Aggrega Kernel        1/24/2081 8:36:36 AM
AcpiPmi      ACPI Power Meter Drive Kernel        11/19/2006 9:20:15 PM
acpitime     ACPI Wake Alarm Driver Kernel        2/9/1974 7:10:30 AM
ADP80XX      ADP80XX                Kernel        4/9/2015 4:49:48 PM
<SNIP>
```

4. ##### Look for KB / Patches installed or not:
```wmic qfe get Caption,Description,HotFixID,InstalledOn```
```
C:\Users\KILLSWITCH-GUI>wmic qfe get Caption,Description,HotFixID,InstalledOn
Caption                                     Description      HotFixID   InstalledOn
http://support.microsoft.com/?kbid=4022405  Update           KB4022405  6/8/2017
http://support.microsoft.com/?kbid=4022730  Security Update  KB4022730  6/8/2017
http://support.microsoft.com/?kbid=4025376  Security Update  KB4025376  7/12/2017
http://support.microsoft.com/?kbid=4025342  Security Update  KB4025342  7/15/2017
<SNIP>
```
```wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:"KB.." /C:"KB.."```
```
C:\Users\KILLSWITCH-GUI> wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:"KB.." /C:"KB4022405"
http://support.microsoft.com/?kbid=4022405  Update           KB4022405  6/8/2017
```

5. ##### Look for unattended configs in the following dirs:
```
c:\sysprep.inf
c:\sysprep\sysprep.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml
```

6. ##### Look for AlwaysInstallElevated key set to DWORD 1:
```reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated```
```
C:\Users\KILLSWITCH-GUI>reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
ERROR: The system was unable to find the specified registry key or value.
```

7. ##### Search the file system for file names containing certain keywords via cmd:
```dir /s *pass* == *cred* == *vnc* == *.config*```
```
C:\Users\KILLSWITCH-GUI>dir /s *pass* == *cred* == *vnc* == *.config*
 Volume in drive C has no label.
 Volume Serial Number is DA67-AFD2

 Directory of C:\Users\KILLSWITCH-GUI\AppData\Local

06/28/2017  09:04 AM    <DIR>          password-app
               0 File(s)              0 bytes
```

8. ##### Search certain file types for a keyword via cmd:
```findstr /si password *.xml *.ini *.txt```
```
C:\Users\KILLSWITCH-GUI>findstr /si password *.xml *.ini *.txt
.PyCharmCE2017.1\config\options\ide.general.xml:    <entry key="ide.ssh.one.time.password" value="true" />
AppData\Local\lxss\rootfs\usr\share\dbus-1\interfaces\org.freedesktop.Accounts.User.xml:  <method name="SetPasswordMode">
```

9. #####  Search for passwords in the reg:
```
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```
```
C:\Users\KILLSWITCH-GUI>reg query HKLM /f password /t REG_SZ /s

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{0fafd998-c8e8-42a1-86d7-7c10c664a415}
    (Default)    REG_SZ    Picture Password Enrollment UX

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{2135f72a-90b5-4ed3-a7f1-8bb705ac276a}
    (Default)    REG_SZ    PicturePasswordLogonProvider
```

10. #####  Search for unquoted service paths:
```wmic service get name,startmode,pathname | findstr /i /v ":\windows\" | findstr /v """```
```
C:\Users\KILLSWITCH-GUI>wmic service get name,startmode,pathname | findstr /i /v ":\windows\" | findstr /v """
Name                                      PathName                                                                                                                                                                                                                                                                             StartMode
AJRouter                                  C:\WINDOWS\system32\svchost.exe -k LocalServiceNetworkRestricted                                                                                                                                                                                                                     Manual
ALG                                       C:\WINDOWS\System32\alg.exe                                                                                                                                                                                                                                                          Manual
AppIDSvc                                  C:\WINDOWS\system32\svchost.exe -k LocalServiceNetworkRestricted    
```

---