


Initial nmap:
```
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain?       syn-ack ttl 127
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to https://10.129.161.179/
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-10-19 10:13:14Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
443/tcp  open  ssl/http      syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Hercules Corp
| tls-alpn:
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=hercules.htb
| Subject Alternative Name: DNS:hercules.htb
| Issuer: commonName=hercules.htb
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
```

web page enumeration

her
```
login                   [Status: 200, Size: 3213, Words: 927, Lines: 54, Duration: 560ms]
                        [Status: 200, Size: 27342, Words: 10179, Lines: 468, Duration: 598ms]
index                   [Status: 200, Size: 27342, Words: 10179, Lines: 468, Duration: 597ms]
default                 [Status: 200, Size: 27342, Words: 10179, Lines: 468, Duration: 582ms]
home                    [Status: 200, Size: 3231, Words: 927, Lines: 54, Duration: 252ms]
content                 [Status: 403, Size: 0, Words: 1, Lines: 1, Duration: 66ms]
Default                 [Status: 200, Size: 27342, Words: 10179, Lines: 468, Duration: 69ms]
Home                    [Status: 200, Size: 3231, Words: 927, Lines: 54, Duration: 63ms]
Index                   [Status: 200, Size: 27342, Words: 10179, Lines: 468, Duration: 66ms]
Login                   [Status: 200, Size: 3213, Words: 927, Lines: 54, Duration: 71ms]
Content                 [Status: 403, Size: 0, Words: 1, Lines: 1, Duration: 75ms]
INDEX                   [Status: 200, Size: 27342, Words: 10179, Lines: 468, Duration: 71ms]
HOME                    [Status: 200, Size: 3231, Words: 927, Lines: 54, Duration: 65ms]

```

```
LDAP hercules.htb 389 DC [+] hercules.htb\ken.w:change*th1s_p@ssw()rd!!
ken.w: change*th1s_p@ssw()rd!!
```

```

<configuration>
    <system.web>
        <compilation debug="false" targetFramework="4.0" />
        <machineKey validationKey="99F1108B685094A8A31CDAA9CBA402028D80C08B40EBBC2C8E4BD4B0D31A347B0D650984650B24828DD120E236B099BFDD491910BF11F6FA915BF94AD93B52BF" decryptionKey="B16DA07AB71AB84143A037BCDD6CFB42B9C34099785C10F9" validation="SHA1" decryption="AES" />
    </system.web>
</configuration
```

```
natalie.a : Prettyprincess123!
```