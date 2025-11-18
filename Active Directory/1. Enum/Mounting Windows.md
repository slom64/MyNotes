
### Show mounts
```sh
showmount -e 10.10.11.78

```

### Mount shares
```sh
mount -t cifs //10.10.10.134/backups /mnt -o user=,password=
```

### Mount nfs
```sh
sudo mount -t nfs 10.10.11.78:/MirageReports /home/slom/HTB/windows/hard/Mirage/mnt -o nolock,vers=3
```

### mount vhd
```sh
guestmount --add /mnt/WindowsImageBackup/L4mpje-PC/Backup\ 2019-02-22\ 124351/9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd --inspector --ro /mnt2/
```