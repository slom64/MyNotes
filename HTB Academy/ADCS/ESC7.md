
---
# 🔹 ESC7 — Vulnerable Certificate Authority Access Control

### 1. The CA itself has permissions

The Enterprise CA object in AD (and its service) has its own set of permissions. Two of the most important rights are:
- **ManageCA** → like being a **CA admin**. Lets you change CA-wide settings.
- **ManageCertificates** → like being a **CA officer / cert manager**. Lets you approve or deny pending cert requests.
---
### 2. Why these rights are dangerous
- **With ManageCA:**
    - You can call methods like `ICertAdminD2::SetConfigEntry`.
    - That lets you flip CA-level flags such as **EDITF_ATTRIBUTESUBJECTALTNAME2**.
    - If you enable that flag → suddenly _all templates_ accept SAN attributes in requests.
    - That turns basically _any cert request into ESC6_.
    - Result: You can request a cert with SAN=Administrator → escalate to DA.
- **With ManageCertificates:**
    - Some templates are configured with **Manager Approval required**.
    - Normally, that means your cert request sits in “Pending” until a CA officer approves it.
    - If you have ManageCertificates, you can approve your _own_ pending requests.
    - That bypasses one of the main safety checks → effectively nullifying manager approval.

---
### 3. ESC7 Abuse Requirements
- If attacker has **ManageCA** → they can enable SAN requests globally → abuse like ESC6.
- If attacker has **ManageCertificates** → they can approve their own or other attacker requests → bypass protections.

---
### 4. Example Flow (ManageCA abuse)
1. Attacker compromises an account with **ManageCA** rights.
2. Attacker flips `EDITF_ATTRIBUTESUBJECTALTNAME2=1`.
3. Now templates that didn’t allow SANs before will accept them.
4. Attacker requests cert with SAN=[administrator@corp.local](mailto:administrator@corp.local).
5. CA issues cert → attacker uses it to logon as DA.

---
### 5. Difference ESC4 vs ESC7
- **ESC4** = Attacker controls **templates** (DACLs on templates in AD).
- **ESC7** = Attacker controls **the CA object itself** (ManageCA / ManageCertificates rights)

👉 ESC4 is “I can edit the blueprints for certs.”  
👉 ESC7 is “I can reconfigure the entire factory or approve certs myself.”

---

### 6. Big Picture

ESC7 is less common than ESC4 in misconfigs, but when it happens it’s **game over**:
- CA admins can make any template vulnerable.
- Cert managers can approve any request.
- Both situations give attackers the ability to mint their own trusted certs → impersonation → domain compromise.
---
⚡ So:
- **ESC4 = Template-level misconfig abuse.**
- **ESC7 = CA-level misconfig abuse.**
---
Want me to also break down **ESC5** next (since it’s another “special case” misconfig, but about enrollment agents again)?