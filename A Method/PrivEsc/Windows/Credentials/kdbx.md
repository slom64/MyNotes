
```sh
john-the-ripper.keepass2john recovery.kdbx > recovery.hash
nano recovery.hash # Remove anything before $keepass
john-the-ripper --format=KeePass  --wordlist ../../rockyou7plus.txt recovery.txt 

#It doesn't work with hashcat, need more investigation.
#It seems the problem is with encoding or something

use the password to open the database using keepassxc, export the file as csv then 
cat userNamesAndPasswords.txt |  cut -d, -f2,4
```