# Testing Server-side parameter pollution 
To test for server-side parameter pollution in the query string, place query syntax characters like #, &, and = in your input and observe how the application responds.
#### Truncating query strings
 You can use a URL-encoded # character after parameter value to attempt to truncate the server-side request. To help you interpret the response, you could also add a string after the # character. 
```
#
#foo
```
Review the response for clues about whether the query has been truncated. For example, if the response returns the user peter, the server-side query may have been truncated. If an Invalid name error message is returned, the application may have treated foo as part of the username. This suggests that the server-side request may not have been truncated.
#### Injecting invalid parameters
You can use an URL-encoded & character to attempt to add a second parameter to the server-side request.
```
&x=y
```
If the response is unchanged this may indicate that the parameter was successfully injected but ignored by the application.
#### Injecting valid parameters
If you're able to modify the query string, you can then attempt to add a second valid parameter to the server-side request. If you've identified the email parameter, you could add it to the query string as follows: 
```
&email=a@a.com
```
#### Overriding existing parameters
To confirm whether the application is vulnerable to server-side parameter pollution, you could try to override the original parameter. Do this by injecting a second parameter with the same name.
```
name=attacker&name=victim
```
 The internal API interprets two name parameters. The impact of this depends on how the application processes the second parameter. This varies across different web technologies. For example:

    PHP parses the last parameter only. This would result in a user search for carlos.
    ASP.NET combines both parameters. This would result in a user search for peter,carlos, which might result in an Invalid username error message.
    Node.js / express parses the first parameter only. This would result in a user search for peter, giving an unchanged result.


