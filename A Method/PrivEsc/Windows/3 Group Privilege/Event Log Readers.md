This group has privileges to read event log of the system, looking at the event log could be tedious because there is alot of data. But the key is enhance your search for specific event logs like searching for succufl

```powershell
Get-WinEvent -FilterHashTable @{logname='security',id=''}  | Get-WinEventData | Select TimeCreated, e_CommandLine | ft -autosize -wrap
```
The `id` represents the event id in windows
- `4740`: A User account was locked out. "Useful in Domain joined computers"
-  `4688`: A new process has been created. "Useful in Non-Domain joined computers" 