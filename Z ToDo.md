
## Web
- [ ] Manual Insecure Deserialization

---
## Red teaming
- [ ] Certificates
- [ ] Active Directory
- [ ] Windows privilege Escelation

---
## Boxes
- [ ] nocturnal
- [ ] cap


```sh
who=$(pwd);curl http://localhost:3000/$who
```

```sh
Buck3tH4TF0RM3!
30 3 * * 0 root test -e /run/systemd/system || SERVICE_MODE=1 /usr/lib/x86_64-linux-gnu/e2fsprogs/e2scrub_all_cron
10 3 * * * root test -e /run/systemd/system || SERVICE_MODE=1 /sbin/e2scrub_all -A -r
/usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
Files with capabilities (limited to 50):                                                                                                                                                           
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
/usr/bin/ping = cap_net_raw+ep              
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep        
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
                                                
╔══════════╣ Checking misconfigurations of ld.so
╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#ldso
/etc/ld.so.conf         
Content of /etc/ld.so.conf:
include /etc/ld.so.conf.d/*.conf
                        
/etc/ld.so.conf.d                               
  /etc/ld.so.conf.d/fakeroot-x86_64-linux-gnu.conf
  - /usr/lib/x86_64-linux-gnu/libfakeroot
  /etc/ld.so.conf.d/libc.conf
  - /usr/local/lib                              
  /etc/ld.so.conf.d/x86_64-linux-gnu.conf
  - /usr/local/lib/x86_64-linux-gnu
  - /lib/x86_64-linux-gnu
  - /usr/lib/x86_64-linux-gnu

/etc/ld.so.preload 

```