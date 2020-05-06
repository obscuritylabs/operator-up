![image](img/facebook_cover_photo_2.png#center)

<p align="center">
    <em>Your one stop resource for operational hints.</em>
</p>

## Table of Contents

### Windows

- [Windows Priv Esc Commands](#windows-priv-esc-commands)
  * [Host Privilege Escalation](#host-privilege-escalation)
    + [Schduled Tasks Path Alteration](#schduled-tasks-path-alteration)
    + [Evaluating Vulnerable Services](#evaluating-vulnerable-services)
    + [Evaluating Vulnerable Drivers](#evaluating-vulnerable-drivers)
    + [Evaluating KBs/Patches](#evaluating-kbs-patches)
    + [Locating Unattended configs](#locating-unattended-configs)
    + [Locating AlwaysInstallElevated](#locating-alwaysinstallelevated)
    + [Locating Sensitive Files](#locating-sensitive-files)
    + [Locating Sensitive Data In Files](#locating-sensitive-data-in-files)
    + [Locating Passwords Within Thhe Registry](#locating-passwords-within-thhe-registry)
    + [Locating Unquoted Service Paths](#locating-unquoted-service-paths)
  * [Domain Privilege Escalation](#domain-privilege-escalation)
    + [Kerbroasting](#kerbroasting)
      - [Kerbroast a domain and set for crashing is hashcat format:](#kerbroast-a-domain-and-set-for-crashing-is-hashcat-format-)
      - [ACL rights to set a SPN on user account and crack via SPN kerb ticket:](#acl-rights-to-set-a-spn-on-user-account-and-crack-via-spn-kerb-ticket-)
- [Windows FYSA Commands](#windows-fysa-commands)
    * [Find all token / user data:](#find-all-token---user-data-)
    * [Get Local ipconfig data:](#get-local-ipconfig-data-)
    * [List all virtual / physical drives with Powershell:](#list-all-virtual---physical-drives-with-powershell-)
    * [Check if host is alive via cmd:](#check-if-host-is-alive-via-cmd-)
    * [List the password policy for the domain:](#list-the-password-policy-for-the-domain-)
    * [Get the current DC your talking to:](#get-the-current-dc-your-talking-to-)
    * [Resolve host-name to IP addr IPv4 with out ping via Powershell:](#resolve-host-name-to-ip-addr-ipv4-with-out-ping-via-powershell-)
    * [Resolve ip to host-name:](#resolve-ip-to-host-name-)
    * [List last boot time via Powershell:](#list-last-boot-time-via-powershell-)


### Linux

### Scanning

- [NMAP Scanning Techniques](#nmap-scanning-techniques)
    * [Internal Host Discovery](#internal-host-discovery)
    * [Internal Full Scope Hit and Run String using Syn Half Scan](#internal-full-scope-hit-and-run-string-using-syn-half-scan)

### Web Ap
- [SQL Map cheat sheet for the wicked](#sql-map-cheat-sheet-for-the-wicked)
  * [SQLMap Optimization](#sqlmap-optimization)
    + [Clone from dev for bleeding edge:](#clone-from-dev-for-bleeding-edge-)
    + [Run SQLMap via a file](#run-sqlmap-via-a-file)
    + [Run from file with threads:](#run-from-file-with-threads-)
    + [Run from file with threads and level:](#run-from-file-with-threads-and-level-)
  * [Tamper all the things:](#tamper-all-the-things-)
    + [General Tamper Testing:](#general-tamper-testing-)
    + [MSSQL Tamper Testing:](#mssql-tamper-testing-)
    + [MySQL Tamper Testing:](#mysql-tamper-testing-)


## What is this project?
