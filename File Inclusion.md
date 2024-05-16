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
#### PHP Session Files
PHP session files can also be used in an LFI attack, leading to Remote Code Execution, particularly if an attacker can manipulate the session data. In a typical web application, session data is stored in files on the server. If an attacker can inject malicious code into these session files, and if the application includes these files through an LFI vulnerability, this can lead to code execution.
##### Exploitation
An attacker could exploit this vulnerability by injecting a PHP code into their session variable by using ```<?php echo phpinfo(); ?>``` in the page parameter.
```
<?php echo phpinfo(); ?>
```
This code is then saved in the session file on the server. Subsequently, the attacker can use the LFI vulnerability to include this session file. Since session IDs are hashed, the ID can be found in the cookies section of your browser. Accessing the Path ```/var/lib/php/sessions/sess_[sessionID]``` will execute the injected PHP code in the session file. Note that you have to replace ```[sessionID]``` with the value from your PHPSESSID cookie.
