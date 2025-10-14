```
KRB5CCNAME=.ccache mssqlclient.py -k -no-pass adminstartor@dc.signed.htb 
```


if you got 
`The login is from an untrusted domain and cannot be used with Integrated authentication.`
Then give it the domain before the username
```
mssqlclient.py 'signed.htb/mssqlsvc:purPLE9795!@@10.129.123.188' -windows-auth
```