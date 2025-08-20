```sh

```

[[Z Assets/Images/Pasted image 20250819081407.jpeg|500]]
![[Z Assets/Images/Pasted image 20250819081407.jpeg]]

```txt

[libdefaults]
        default_realm = DUMMY.REALM
        dns_lookup_kdc = true
        dns_lookup_realm = true

[realms]
        DUMMY.REALM = {
                kdc = {{dc_hostname}}.{{realm_placeholder}}
                admin_server = {{dc_hostname}}.{{realm_placeholder}}
                default_domain = {{realm_placeholder}}
        }

[domain_realm]
        .{{realm_placeholder}} = {{realm_placehoder}}
        {{realm_placeholder}} = {{realm_placehoder}}


____________

[libdefaults]
        default_realm = DUMMY.REALM
        dns_lookup_kdc = true
        dns_lookup_realm = true

# The following krb5.conf variables are only for MIT Kerberos.
        kdc_timesync = 1
        ccache_type = 4
        forwardable = true
        proxiable = true
        rdns = false
        dns_lookup_kdc = true
        dns_lookup_realm = true

# The following libdefaults parameters are only for Heimdal Kerberos.
        fcc-mit-ticketflags = true

[realms]
        DUMMY.REALM = {
                kdc = dummy.local
                admin_server = dummy.local
        }

[domain_realm]
        .puppy.htb = PUPPY.HTB
        puppy.htb = PUPPY.HTB
        .mit.edu = ATHENA.MIT.EDU
        mit.edu = ATHENA.MIT.EDU



```


```sh
faketime "$( ntpdate -q voleur.htb | cut -d ' ' -f 1,2)" evil-winrm -i dc.voleur.htb -k -u svc_winrm -r voleur.htb
```