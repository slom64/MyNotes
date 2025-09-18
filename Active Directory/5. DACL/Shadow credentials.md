 **Shadow Credentials** is an Active Directory persistence/privilege-escalation technique where an attacker **adds a valid key/certificate to a user or computer AD object** (the `msDS-KeyCredentialLink` attribute). Once that key is present the attacker can authenticate as the target via certificate/PKINIT (passwordless), get Kerberos tickets and move laterally — without needing the account password. [posts.specterops.io+1](https://posts.specterops.io/shadow-credentials-abusing-key-trust-account-mapping-for-takeover-8ee1a53566ab?utm_source=chatgpt.com)
# How it works (conceptually)
- Windows (Windows Hello for Business / key-trust flows) stores public key credential entries on AD objects in the `msDS-KeyCredentialLink` attribute. Those entries let the object authenticate using the corresponding private key.
- If an attacker can **modify** that attribute (or otherwise append a key blob there) they can register a key they control for the victim account. The attacker then uses the private key to perform certificate-based Kerberos authentication (PKINIT) and obtain TGT/STs as the victim.

## Preconditions & attack surface
- The attacker needs **write control** over the target AD object (or an AD misconfiguration/DACL that allows appending to `msDS-KeyCredentialLink`). This often requires elevated AD permissions or a DACL/ACL weakness on the user/computer object.
- The environment must accept certificate/PKINIT authentication for the account (many do, especially when Windows Hello for Business or certificate authentication is in use).

