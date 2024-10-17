Server-Side Parameter Pollution (SSPP) is a web vulnerability where an attacker injects multiple parameters with the same name into an HTTP request. The server may interpret these parameters in unexpected ways, potentially leading to security issues such as unauthorized access, privilege escalation, or bypassing security controls.

### Testing for Server-Side Parameter Pollution

1. **Duplicate Parameters Handling:**
   - Send multiple parameters with the same name and different values to see how the server processes them. Different servers or frameworks may use the first, last, or a combination of parameter values.
   
2. **Parameter Injection in Different Contexts:**
   - Try injecting additional parameters into the URL, query string, body, or headers to see how they interact with existing parameters.

3. **Interfering with Security or Business Logic:**
   - Manipulate parameters to change roles, privileges, or other security-sensitive data.

### Example Scenarios with HTTP Requests

#### Scenario 1: Manipulating User Roles (Privilege Escalation)
Assume there is an endpoint that allows a user to update their profile information, and there is a parameter named `role` that determines the user's role.

**Original Request:**
```
POST /updateProfile
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=john&role=user
```

An attacker can attempt SSPP by injecting a second `role` parameter with a higher privilege.

**Manipulated Request:**
```
POST /updateProfile
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=john&role=user&role=admin
```

If the server takes the last parameter's value or combines them, the attacker may gain admin privileges.

#### Scenario 2: Bypassing Payment Validation
In an e-commerce platform, assume there is an endpoint to process payments, where the `amount` parameter indicates the transaction amount.

**Original Request:**
```
POST /processPayment
Host: shop.com
Content-Type: application/x-www-form-urlencoded

amount=100
```

By adding another `amount` parameter with a different value, an attacker can see if the server uses a different value.

**Manipulated Request:**
```
POST /processPayment
Host: shop.com
Content-Type: application/x-www-form-urlencoded

amount=100&amount=1
```

If the server accepts the second `amount` value, the attacker may pay only 1 unit instead of 100.

#### Scenario 3: URL Parameter Injection (Overriding Default Behavior)
An application might use a URL parameter to control access to different functionalities, such as `action=view` or `action=edit`.

**Original Request:**
```
GET /document?action=view&id=123
Host: documents.com
```

An attacker tries injecting another `action` parameter to see if it changes the server's behavior.

**Manipulated Request:**
```
GET /document?action=view&action=edit&id=123
Host: documents.com
```

If the server uses the second `action` parameter, the attacker might gain edit permissions.

### Scenario 4: Truncating query strings

You can use a URL-encoded # character to attempt to truncate the server-side request. To help you interpret the response, you could also add a string after the # character.

**Original Request**
```
GET /userSearch?name=peter&back=/home
```
**Manipulated Request:**
```
GET /users/search?name=peter#&publicProfile=true
```
### Scenario 5: Structured Data Formats
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
POST /myaccount
```name=peter","access_level":"administrator```

If the user input is added to the server-side JSON data without adequate validation or sanitization, this results in the following server-side request:
```
PATCH /users/7312/update
{name="peter","access_level":"administrator"}
```
This may result in the user peter being given administrator access.
