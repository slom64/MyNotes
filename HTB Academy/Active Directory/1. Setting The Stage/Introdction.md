
Throughout the module, we will cover examples with accompanying command output. Most of which can be reproduced on the target VMs that can be spawned within the relevant sections. You will be provided RDP credentials to interact with some of the target VMs to learn how to enumerate and attack from a Windows host (`MS01`) and SSH access to a preconfigured Parrot Linux host (`ATTACK01`) to perform enumeration and attack examples from Linux. You can connect from the Pwnbox or your own VM (after downloading a VPN key once a machine spawns) via RDP using [FreeRDP](https://github.com/FreeRDP/FreeRDP/wiki/CommandLineInterface), [Remmina](https://remmina.org/), or the RDP client of your choice where applicable or the SSH client built into the Pwnbox or your own VM.

#### Connecting via FreeRDP

We can connect via command line using the command:

```shell-session
slomkm@htb[/htb]$ xfreerdp /v:<MS01 target IP> /u:htb-student /p:Academy_student_AD!
```

#### Connecting via SSH

We can connect to the provided Parrot Linux attack host using the command, then enter the provided password when prompted.

```shell-session
slomkm@htb[/htb]$ ssh htb-student@<ATTACK01 target IP>
```

#### Xfreerdp to the ATTACK01 Parrot Host

We also installed an `XRDP` server on the `ATTACK01` host to provide GUI access to the Parrot attack host. This can be used to interact with the BloodHound GUI tool which we will cover later in this section. In sections where this host spawns (where you are given SSH access) you can also connect to it using `xfreerdp` using the same command as you would with the Windows attack host above:

```shell-session
slomkm@htb[/htb]$ xfreerdp /v:<ATTACK01 target IP> /u:htb-student /p:HTB_@cademy_stdnt!
```

Most sections will provide credentials for the `htb-student` user on either `MS01` or `ATTACK01`. Depending on the material and challenges, some sections will have you authenticate to a target with a different user, and alternate credentials will be provided.