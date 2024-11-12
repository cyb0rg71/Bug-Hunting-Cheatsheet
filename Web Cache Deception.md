## Description

Web cache deception is a vulnerability that enables an attacker to trick a web cache into storing sensitive, dynamic content. It's caused by discrepancies between how the cache server and origin server handle requests. In a web cache deception attack, an attacker persuades a victim to visit a malicious URL, inducing the victim's browser to make an ambiguous request for sensitive content. The cache misinterprets this as a request for a static resource and stores the response. The attacker can then request the same URL to access the cached response, gaining unauthorized access to private information.

## Cache Rules

Web cache deception attacks exploit how cache rules are applied, so it's important to know about some different types of rules, particularly those based on defined strings in the URL path of the request. For example:

    1. Static file extension rules - These rules match the file extension of the requested resource, for example .css for stylesheets or .js for JavaScript files.
    2. Static directory rules - These rules match all URL paths that start with a specific prefix. These are often used to target specific directories that contain only static resources, for example /static or /assets.
    3. File name rules - These rules match specific file names to target files that are universally required for web operations and change rarely, such as robots.txt and favicon.ico.

## Constructing a web cache deception attack

Constructing a basic web cache deception attack involves the following steps:

    1. Identify a target endpoint that returns a dynamic response containing sensitive information. Review responses in Burp, as some sensitive information may not be visible on the rendered page. Focus on endpoints that support the GET, HEAD, or OPTIONS methods as requests that alter the origin server's state are generally not cached.

    2. Identify a discrepancy in how the cache and origin server parse the URL path. This could be a discrepancy in how they:
        Map URLs to resources.
        Process delimiter characters.
        Normalize paths.
    3. Craft a malicious URL that uses the discrepancy to trick the cache into storing a dynamic response. When the victim accesses the URL, their response is stored in the cache. Using Burp, you can then send a request to the same URL to fetch the cached response containing the victim's data. Avoid doing this directly in the browser as some applications redirect users without a session or invalidate local data, which could hide a vulnerability.
