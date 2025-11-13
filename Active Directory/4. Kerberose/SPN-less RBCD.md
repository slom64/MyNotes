
In a normal Resource-Based Constrained Delegation attack, the service `WEBSRV` will initiate a `TGS-REQ` with the `S4U2Self` Kerberos extesnsion, asking the KDC to Impersonate user `DC01` to access itself.
If protocol transition is enabled, the `WEBSRV` service will get a `forwardable service ticket` for the user `DC01` to be used by the itself to access itself.
The `WEBSRV` then will initiate another `TGS-REQ` to Impersonate that user to access `DBSRV`, but this time with the `S4U2Proxy`, embedding a copy of the previously fetched `service ticket` for user `DC01`.
The KDC, will try to decrypt the `service ticket` sent by `WEBSRV` using the service's secret key, if succeeded, the KDC can then extract the embedded `service ticket` of `DC01`, and check if the service `WEBSRV` is allowed to Impersonate users to access `DBSRV`, if it is, then the KDC will send back a `service ticket` with `DC01` user's info, encrypted with `DBSRV` secret key, for `WEBSRV` to use to Impersonate that user and access `DBSRV`.

Now, to SPN-less RBCD Attacks.
If user `ahmed` wants to Impersonate `DC01` to access `DBSRV`, he will try to follow the same flow, `ahmed` will initiate a `TGS-REQ` with the `S4U2Self` Kerberos extension, asking the KDC to Impersonate user `DC01` to access itself.
The KDC will have no Idea which key to use to encrypt the `service ticket`, so it will fail in this case.
`ahmed`, to make this attack succeed, will use `S4U2Self` + `U2U` Kerberos Extensions.
`U2U` will tell the KDC:

> Encrypt the service ticket using ahmed's Ticket Session Key from his TGT

The KDC will encrypt the service ticket using ahmed's Ticket Session Key from his TGT, and sent it back to ahmed.
Now, when ahmed wants to go on with the attack, he will try to send a TGS-REQ with S4U2Proxy Kerberos Extension, embedding a copy of DC01 previously acquired service ticket in the request.
When the KDC receives the TGS-REQ, it will try to decrypt it using ahmed's secret key, as it has no Idea this was issued using the U2U Kerberos Extension, and the KDC will fail here.
To make this work, ahmed changes his password hash to the Ticket Session Key, in this case, when ahmed try again to request a TGS-REQ with S4U2Proxy Kerberos extension, the KDC will try to decrypt it with ahmed's secret key and will succeed, then the KDC will check if ahmed has permissions to Impersonate DC01 to access DBSRV, and if yes, it will send back a service ticket with DC02 user info, which ahmed will then use to access DBSRV

Limitations for this attack is:
 1. RC4 must be allowed for Kerberos,
 2. the attack will render the account used for it inaccessible,
 3. domain policy might prevent password reset without the intervention ofa privileged account die to minimum password age requirements