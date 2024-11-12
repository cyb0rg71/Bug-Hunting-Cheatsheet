## Description

Web cache deception is a vulnerability that enables an attacker to trick a web cache into storing sensitive, dynamic content. It's caused by discrepancies between how the cache server and origin server handle requests. In a web cache deception attack, an attacker persuades a victim to visit a malicious URL, inducing the victim's browser to make an ambiguous request for sensitive content. The cache misinterprets this as a request for a static resource and stores the response. The attacker can then request the same URL to access the cached response, gaining unauthorized access to private information.

## Cache Rules

Web cache deception attacks exploit how cache rules are applied, so it's important to know about some different types of rules, particularly those based on defined strings in the URL path of the request. For example:

    1. Static file extension rules - These rules match the file extension of the requested resource, for example .css for stylesheets or .js for JavaScript files.
    2. Static directory rules - These rules match all URL paths that start with a specific prefix. These are often used to target specific directories that contain only static resources, for example /static or /assets.
    3. File name rules - These rules match specific file names to target files that are universally required for web operations and change rarely, such as robots.txt and favicon.ico.
