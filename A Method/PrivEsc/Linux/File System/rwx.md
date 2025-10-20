```sh
find / -type f -writable 2>/dev/null | grep -v -E '/proc/|/run/|/sys/|/dev/' #-readable 
```

### Find Files Writable by a Specific Group
```sh
find / -group groupname -writable 2>/dev/null
```

```sh
find / -group groupname -perm -g=w 2>/dev/null
```
### Find Files Where User Has Write Permissions (Regardless of Owner)

```sh
find / -perm -u=w -user username 2>/dev/null
```
### 4. Find Files Where Group Has Write Permissions
```sh
find / -perm -g=w -group groupname 2>/dev/null
```

### More Comprehensive Search (Combining Multiple Conditions)

```sh
find / -type f \( -user username -perm -u=w \) -o \( -group groupname -perm -g=w \) 2>/dev/null
```

### Search Only in Important Directories (Faster Scan)
```sh
find /etc /var /tmp /home /usr -group groupname -writable 2>/dev/null
```
### 7. List World-Writable Files (Writable by Anyone)
```sh
find / -perm -o=w ! -type l 2>/dev/null
```
### 8. Find World-Writable Directories

```sh
find / -type d -perm -o=w ! -perm -o=t 2>/dev/null
```