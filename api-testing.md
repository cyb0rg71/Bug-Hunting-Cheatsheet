# Api Recon
    Discovering API documentation
    Fuzzing
    Burp Scanner
    Changing http methods
#### Mass Assignment
Mass assignment typically involves setting multiple attributes of an object in a single call. For instance, in a web application, a user might submit a form that includes fields for various attributes of a user profile, such as name, email, and role.

If the application doesn't properly validate or sanitize the incoming data, an attacker could potentially manipulate the input to include additional attributes or modify existing ones. This could lead to unauthorized changes to the data model or allow an attacker to escalate privileges within the application.

For example, consider a web application that allows users to update their profile information. If the application is vulnerable to mass assignment, an attacker might be able to include a "isAdmin" field in the form submission and set it to "true", thereby gaining administrative privileges.
When fuzzing, use wordlists based on common API naming conventions and industry terms. 
# Testing Server-side parameter pollution 
To test for server-side parameter pollution in the query string, place query syntax characters like #, &, and = in your input and observe how the application responds.
### Truncating query strings
 You can use a URL-encoded # character after parameter value to attempt to truncate the server-side request. To help you interpret the response, you could also add a string after the # character. 
```
#
#foo
```
Review the response for clues about whether the query has been truncated. For example, if the response returns the user peter, the server-side query may have been truncated. If an Invalid name error message is returned, the application may have treated foo as part of the username. This suggests that the server-side request may not have been truncated.
### Injecting invalid parameters
You can use an URL-encoded & character to attempt to add a second parameter to the server-side request.
```
&x=y
```
If the response is unchanged this may indicate that the parameter was successfully injected but ignored by the application. To build up a more complete picture, you'll need to test further. 
#### Injecting valid parameters
If you're able to modify the query string, you can then attempt to add a second valid parameter to the server-side request. If you've identified the email parameter, you could add it to the query string as follows: 
```
name=attacker&email=a@a.com
```
### Overriding existing parameters
To confirm whether the application is vulnerable to server-side parameter pollution, you could try to override the original parameter. Do this by injecting a second parameter with the same name.
```
name=attacker&name=victim
```
 The internal API interprets two name parameters. The impact of this depends on how the application processes the second parameter. This varies across different web technologies. For example:

    PHP parses the last parameter only. This would result in a user search for carlos.
    ASP.NET combines both parameters. This would result in a user search for peter,carlos, which might result in an Invalid username error message.
    Node.js / express parses the first parameter only. This would result in a user search for peter, giving an unchanged result.
If you're able to override the original parameter, you may be able to conduct an exploit. For example, you could add name=administrator to the request. This may enable you to log in as the administrator user. 
### Testing for server-side parameter pollution in REST paths
A RESTful API may place parameter names and values in the URL path, rather than the query string. An attacker may be able to manipulate server-side URL path parameters to exploit the API. To test for this vulnerability, add path traversal sequences to modify parameters and observe how the application responds.
```
name=carlos/../../../../../ >> To navigate the api definition.
name=../../../../openapi.json%23 >> Add or bruteforce some common API definition filenames to the URL path.
```
### Testing for server-side parameter pollution in structured data formats
 Consider an application that enables users to edit their profile, then applies their changes with a request to a server-side API. When you edit your name, your browser makes the following request:
```
POST /myaccount
name=peter
```
This results in the following server-side request:
```
PATCH /users/7312/update
{"name":"peter"}
```
You can attempt to add the access_level parameter to the request as follows:
```
POST /myaccount
name=peter","access_level":"administrator
```
If the user input is added to the server-side JSON data without adequate validation or sanitization, this results in the following server-side request:
```
PATCH /users/7312/update
{name="peter","access_level":"administrator"}
```
This may result in the user peter being given administrator access. 
#### ____
Consider a similar example, but where the client-side user input is in JSON data. When you edit your name, your browser makes the following request:
```
POST /myaccount
{"name": "peter"}
```
This results in the following server-side request:
```
PATCH /users/7312/update
{"name":"peter"}
```
You can attempt to add the access_level parameter to the request as follows:
```
POST /myaccount
{"name": "peter\",\"access_level\":\"administrator"}
```
If the user input is decoded, then added to the server-side JSON data without adequate encoding, this results in the following server-side request:
```
PATCH /users/7312/update
{"name":"peter","access_level":"administrator"}
```
Again, this may result in the user peter being given administrator access.

Structured format injection can also occur in responses. For example, this can occur if user input is stored securely in a database, then embedded into a JSON response from a back-end API without adequate encoding. You can usually detect and exploit structured format injection in responses in the same way you can in requests. This examples are in JSON, but server-side parameter pollution can occur in any structured data format. For an example in XML, see the XInclude attacks section in the XML external entity (XXE) injection topic.
