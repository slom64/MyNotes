https://www.synacktiv.com/publications/ounedpy-exploiting-hidden-organizational-units-acl-attack-vectors-in-active-directory
- Every **OU object** in AD has an attribute called **`gPLink`**, which is basically a list of **Group Policy Objects (GPOs)** linked to it.
- If you have **`WriteGPlink`**, you can **add, remove, or reorder GPO links** on that OU.
### ⚡ Impact

1. **Linking Malicious GPOs**
    - You can link an existing GPO (that you control) to the OU.
    - Any computers or users in that OU will start applying that GPO automatically.
    👉 Example: You could push a startup script that runs `net localgroup administrators attacker /add` on all machines in that OU.
2. **Privilege Escalation**
    - If the OU contains **privileged users or computers** (like IT staff, servers, or even a Domain Controller if misconfigured), you could gain **Domain Admin** indirectly.
3. **Persistence**
    - You could add a GPO that re-adds your backdoor user to local admins on reboot.
    - Even if defenders try to remove you manually, GPO will reapply.
4. **Indirect Abuse**
    - You don’t even need `Write` access to the GPO itself. Just the ability to **link** an already existing “bad” GPO.
    - Sometimes attackers create a GPO if they also have `CreateGPO` rights, then link it via `WriteGPlink`.
---
### ✅ Example Attack Path
Let’s say:

- You compromise a low-privileged service account.    
- That account has `WriteGPlink` on `OU=Workstations`.
- You create (or reuse) a GPO that runs a PowerShell reverse shell on logon.
- You link it to that OU.

👉 Next time a workstation user logs in → your payload executes → creds pop → escalate further.

---
### ⚡ So in short:  
`WriteGPlink` = ability to **force policy execution on all users/computers in that OU** → huge attack surface → often leads to privilege escalation.