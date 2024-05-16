#### PHP Wrappers
PHP wrappers are part of PHP's functionality that allows users access to various data streams. Wrappers can also access or execute code through built-in PHP protocols, which may lead to significant security risks if not properly handled.
```php
php://filter/convert.base64-encode/resource=/etc/passwd
```
Once the application processes this payload, the server will return an encoded content of the passwd file. Then decode the base64 encoded data.
#### Data Wrappers
The data stream wrapper is another example of PHP's wrapper functionality. The ```data://``` wrapper allows inline data embedding. It is used to embed small amounts of data directly into the application code.
```txt
data:text/plain,<?php%20phpinfo();%20?>
```
This Payload could cause PHP code execution, displaying the PHP configuration details.
