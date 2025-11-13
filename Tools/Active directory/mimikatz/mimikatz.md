```powershell
### BUSYLIGHT Module

- busylight::list - Provides additional information for and control of connected BusyLights.
- busylight::off - Turns off the BusyLight.
- busylight::single - Activates a single BusyLight.
- busylight::status - Displays the status of BusyLights.
- busylight::test - Tests the BusyLight functionality.

### CRYPTO Module

- crypto::capi (experimental) - Patches CryptoAPI layer for easy export of certificates.
- crypto::certificates - Lists and exports certificates (typically requires privilege::debug).
    - /systemstore (optional) - Specifies the system store to use (default: CERT_SYSTEM_STORE_CURRENT_USER).
    - /store (optional) - Specifies the store to use (default: My); list with crypto::stores.
    - /export (optional) - Exports all certificates to files (public parts in DER, private parts in PFX files - password protected with: mimikatz).
- crypto::certtohw - Tries to export a software CA to a crypto (virtual) hardware.
- crypto::cng (experimental) - Patches CNG service for easy export (patches “KeyIso” service).
- crypto::extract (experimental) - Extracts keys from CAPI RSA/AES provider.
- crypto::hash - Hashes a password with an optional username.
- crypto::keys - Lists and exports key containers.
- crypto::providers - Lists cryptographic providers.
- crypto::sc - Lists smartcard readers.
- crypto::scauth - Creates an authentication certificate (smartcard-like) from a CA.
- crypto::stores - Lists cryptographic stores.
    - /systemstore (optional) - Specifies the system store to use (default: CERT_SYSTEM_STORE_CURRENT_USER).
- crypto::system - Describes a Windows System Certificate (file, TODO: registry or hive).

### DPAPI Module

- dpapi::blob - Unprotects a DPAPI blob with API or Masterkey.
- dpapi::cache - Tests DPAPI cache.
- dpapi::capi - Tests CAPI key.
- dpapi::chrome - Tests Chrome key.
- dpapi::cng - Tests CNG key.
- dpapi::cred - Tests CREd.
- dpapi::credhist - Configures a Credhist file.
- dpapi::masterkey - Configures a Masterkey file and unprotects (key depending).
- dpapi::protect - Protects data using DPAPI.
- dpapi::vault - Tests VAULT.
- dpapi::wifi - Tests WIFI (XML profile required).
- dpapi::wwan - Tests WWAN (XML profile required).

### EVENT Module

- event::clear - Clears an event log.
- event::drop (experimental) - Patches Events service to avoid new events.

### IIS Module

- iis::apphost - Handles IIS XML Config.

### KERBEROS Module

- kerberos::ask - Requests TGS tickets.
- kerberos::clist - Lists tickets in MIT/Heimdall ccache.
- kerberos::golden - Creates golden/silver/trust tickets.
    - /domain - Fully qualified domain name.
    - /sid - SID of the domain.
    - /sids (optional) - Additional SIDs for spoofing (e.g., Enterprise Admins).
    - /user - Username to impersonate.
    - /groups (optional) - Group RIDs (default: 513,512,520,518,519).
    - /krbtgt - NTLM hash for KRBTGT.
    - /ticket (optional) - Path to save ticket.
    - /ptt - Injects ticket into memory.
    - /id (optional) - User RID (default: 500).
    - /startoffset (optional) - Start offset (default: 0).
    - /endin (optional) - Ticket lifetime (default: 10 years).
    - /renewmax (optional) - Maximum renewal lifetime (default: 10 years).
    - /aes128 - AES128 key.
    - /aes256 - AES256 key.
    - For Silver Ticket: /target, /service, /rc4 required.
    - For Trust Ticket: /target, /service, /rc4 required.
- kerberos::hash - Hashes password to keys.
- kerberos::list - Lists all user tickets (TGT and TGS) in user memory (similar to klist).
    - /export - Exports user tickets to files.
- kerberos::ptc - Passes the cache (NT6) - Injects Kerberos tickets from ccache files.
- kerberos::ptt - Passes the ticket.
    - /filename - Ticket filename (can be multiple).
    - /directory - Directory path to inject all .kirbi files.
- kerberos::purge - Purges all Kerberos tickets.
- kerberos::tgt - Gets current TGT for current user.

### LSADUMP Module

- lsadump::backupkeys - Gets preferred backup master keys (requires Administrator rights).
- lsadump::cache - Gets SysKey to decrypt NL$KM and MSCache(v2) (requires Administrator rights).
- lsadump::changentlm - Asks a server to set a new password/NTLM for one user.
- lsadump::dcshadow - Pushes replication changes to a Domain Controller (requires full AD admin or KRBTGT hash).
- lsadump::dcsync - Asks a DC to synchronize an object (get password data for account).
    - /all - Pulls data for entire domain.
    - /user - User ID or SID.
    - /domain (optional) - FQDN of domain (defaults to current).
    - /csv - Exports to CSV.
    - /dc (optional) - Specifies Domain Controller.
- lsadump::lsa - Asks LSA Server to retrieve SAM/AD enterprise.
    - /inject - Injects LSASS to extract credentials.
    - /name - Account name.
    - /id - RID for target user.
    - /patch - Patches LSASS.
- lsadump::netsync - Uses DC computer account password to impersonate DC via Silver Ticket and DCSync.
- lsadump::rpdata - Gets RpData.
- lsadump::sam - Gets SysKey to decrypt SAM entries (requires System or Debug rights).
- lsadump::secrets - Gets SysKey to decrypt SECRETS entries (requires System or Debug rights).
- lsadump::setntlm - Asks a server to set a new password/NTLM for one user.
- lsadump::trust - Asks LSA Server to retrieve Trust Auth Information (requires System or Debug rights).

### MISC Module

- misc::addsid - Adds to SIDHistory (moved to sid::modify as of May 6th, 2016).
- misc::cmd - Command Prompt (requires Administrator rights).
- misc::compressme - Compresses Mimikatz file.
- misc::detours (experimental) - Enumerates modules with Detours-like hooks (requires Administrator rights).
- misc::memssp - Injects malicious Windows SSP to log credentials (requires Administrator rights).
- misc::mflt - Gathers details on loaded drivers (available in v2.1.1).
- misc::ncroutemon - Juniper Manager (without DisableTaskMgr).
- misc::regedit - Registry Editor (requires Administrator rights).
- misc::skeleton - Injects Skeleton Key into LSASS (requires Administrator rights).
- misc::taskmgr - Task Manager (requires Administrator rights).
- misc::wifi - No longer in MISC (moved to dpapi::wifi).

### MINESWEEPER Module

- minesweeper::infos - Provides mine info in Minesweeper.

### NET Module

- net::alias - Manages network aliases.
- net::group - Manages network groups.
- net::serverinfo - Displays server information.
- net::session - Manages sessions.
- net::share - Manages shares.
- net::stats - Displays statistics.
- net::tod - Displays time of day.
- net::user - Manages users.
- net::wsession - Manages workstation sessions.

### PRIVILEGE Module

- privilege::backup - Gets backup privilege (requires Debug rights).
- privilege::debug - Gets debug rights (required for many commands; Administrators group has it by default).
- privilege::driver - Gets driver privilege (requires Debug rights).
- privilege::id - Gets privilege by ID (requires Debug rights).
- privilege::name - Gets privilege by name (requires Debug rights).
- privilege::restore - Gets restore privilege (requires Debug rights).
- privilege::security - Gets security privilege (requires Debug rights).
- privilege::sysenv - Gets system environment privilege (requires Debug rights).
- privilege::tcb - Gets TCB privilege (requires elevated rights).

### PROCESS Module

- process::exports - Lists exports.
- process::imports - Lists imports.
- process::list - Lists running processes (requires Administrator rights).
- process::resume - Resumes a process.
- process::run - Runs a process.
- process::start - Starts a process.
- process::stop - Terminates a process.
- process::suspend - Suspends a process.

### RPC Module

- rpc::close - Closes RPC connection.
- rpc::connect - Connects to RPC.
- rpc::enum - Enumerates RPC.
- rpc::server - Manages RPC server.

### SEKURLSA Module

- sekurlsa::backupkeys - Gets preferred backup master keys.
- sekurlsa::credman - Lists Credentials Manager.
- sekurlsa::dpapi - Lists cached MasterKeys.
- sekurlsa::dpapisystem - DPAPI_SYSTEM secret.
- sekurlsa::ekeys - Lists Kerberos encryption keys.
- sekurlsa::kerberos - Lists Kerberos credentials for all users.
- sekurlsa::krbtgt - Gets KRBTGT password data.
- sekurlsa::livessp - Lists LiveSSP credentials.
- sekurlsa::logonpasswords - Lists all provider credentials (requires debug or SYSTEM).
- sekurlsa::minidump - Switches to LSASS minidump context.
- sekurlsa::msv - Lists LM & NTLM credentials.
- sekurlsa::process - Switches to LSASS process context.
- sekurlsa::pth - Pass-the-Hash and Over-Pass-the-Hash.
    - /user - Username to impersonate.
    - /domain - Domain name.
    - /rc4 or /ntlm - NTLM hash.
    - /run (optional) - Command to run (default: cmd).
- sekurlsa::ssp - Lists SSP credentials.
- sekurlsa::tickets - Lists all Kerberos tickets.
    - /export - Exports tickets to .kirbi files.
- sekurlsa::trust - Gets trust keys (deprecated in favor of lsadump::trust /patch).
- sekurlsa::tspkg - Lists TsPkg credentials.
- sekurlsa::wdigest - Lists WDigest credentials.

### SERVICE Module

- service::+ - Installs Mimikatz service (‘mimikatzsvc’).
- service::- - Uninstalls Mimikatz service (‘mimikatzsvc’).
- service::list - Lists services.
- service::me - Displays service info.
- service::preshutdown - Preshutdown service.
- service::remove - Removes service.
- service::resume - Resumes service.
- service::shutdown - Shuts down service.
- service::start - Starts service.
- service::stop - Stops service.
- service::suspend - Suspends service.

### SID Module

- sid::add - Adds SID to SIDHistory.
- sid::clear - Clears SIDHistory.
- sid::lookup - Looks up by name (/name) or SID (/sid).
- sid::modify - Modifies object SID.
- sid::patch - Patches NTDS service.
- sid::query - Queries object by SID or name.

### STANDARD Module

- standard::answer - Answers the Ultimate Question.
- standard::base64 - Switches output to base64.
- standard::cd - Changes or displays current directory.
- standard::cls - Clears screen.
- standard::coffee - Shows ASCII coffee image.
- standard::exit - Quits Mimikatz.
- standard::hostname - Displays system hostname.
- standard::localtime - Displays system local date and time.
- standard::log - Sends Mimikatz data to log file.
- standard::sleep - Sleeps for milliseconds.
- standard::version - Displays version information.
```