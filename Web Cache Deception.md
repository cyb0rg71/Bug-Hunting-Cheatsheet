# Web Cache Deception

## Description

Web cache deception is a vulnerability that enables an attacker to trick a web cache into storing sensitive, dynamic content. It's caused by discrepancies between how the cache server and origin server handle requests. In a web cache deception attack, an attacker persuades a victim to visit a malicious URL, inducing the victim's browser to make an ambiguous request for sensitive content. The cache misinterprets this as a request for a static resource and stores the response. The attacker can then request the same URL to access the cached response, gaining unauthorized access to private information.

---

## Cache Keys

Cache keys are the URL path and query parameters. If the incoming request's cache key matches that of a previous request, the cache considers them to be equivalent and serves a copy of the cached response.

---

## Cache Rules

Web cache deception attacks exploit how cache rules are applied, so it's important to know about some different types of rules, particularly those based on defined strings in the URL path of the request.

### **1. Exploiting Static File Extension Rules**

#### **Description**:
Static file extension rules apply caching based on file extensions such as `.css`, `.js`, `.jpg`, etc. An attacker can craft URLs with static extensions to deceive the caching layer.

#### **Example**:
- **Target URL**: `/profile/` (Dynamic and user-specific content).
- **Exploited URL**: `/profile/.css` (Cache interprets it as static CSS content).

#### **Steps**:
1. Access `/profile/.css` while logged in as a user.
2. The server processes `/profile/` and serves private profile data, but the cache stores the response assuming it is static CSS.
3. Any subsequent request to `/profile/.css` will serve cached sensitive data to other users.

---

### **2. Exploiting Static Directory Rules**

#### **Description**:
Static directory rules match all URLs within specific directories (e.g., `/static/`, `/assets/`). If sensitive content is accessible via these paths, it may get improperly cached.

#### **Example**:
- **Target URL**: `/user/profile` (Sensitive dynamic content).
- **Exploited URL**: `/static/user/profile` or `/assets/user/profile`.

#### **Steps**:
1. Append or prepend a static directory prefix, e.g., `/static/user/profile`.
2. Send a request while authenticated. The server processes it normally, but the cache sees the URL as belonging to a static directory and caches the content.
3. Access `/static/user/profile` without authentication and retrieve cached sensitive content.

---

### **3. Exploiting File Name Rules**

#### **Description**:
File name rules match specific files (e.g., `favicon.ico`, `robots.txt`) that are assumed to be universally static. Exploiting this trust can lead to cache poisoning.

#### **Example**:
- **Target URL**: `/user/profile` (Sensitive user-specific content).
- **Exploited URL**: `/user/profile/robots.txt`.

#### **Steps**:
1. Request `/user/profile/robots.txt` while authenticated.
2. The server processes `/user/profile` and serves sensitive content, but the cache stores it as a static file named `robots.txt`.
3. Anyone accessing `/user/profile/robots.txt` retrieves the cached sensitive content.

---

### **Combined Exploits**

Attackers often combine these techniques for broader impact:
1. **File Extension + Static Directory**: `/static/user/profile.jpg`.
2. **File Name + Directory**: `/assets/user/profile/favicon.ico`.

---

## Real-World Example

- **Scenario**:
  - An online banking application has a user-specific dashboard at `/dashboard/`.
  - The cache rules include:
    - Static extensions: `.css`, `.js`.
    - Static directory: `/assets/`.
    - File names: `favicon.ico`.

---

## Constructing a Web Cache Deception Attack

Constructing a basic web cache deception attack involves the following steps:

1. Identify a target endpoint that returns a dynamic response containing sensitive information. Review responses in Burp, as some sensitive information may not be visible on the rendered page. Focus on endpoints that support the GET, HEAD, or OPTIONS methods as requests that alter the origin server's state are generally not cached.

2. Identify a discrepancy in how the cache and origin server parse the URL path. This could be a discrepancy in how they:
    - Map URLs to resources.
    - Process delimiter characters.
    - Normalize paths.

3. Craft a malicious URL that uses the discrepancy to trick the cache into storing a dynamic response. When the victim accesses the URL, their response is stored in the cache. Using Burp, you can then send a request to the same URL to fetch the cached response containing the victim's data. Avoid doing this directly in the browser as some applications redirect users without a session or invalidate local data, which could hide a vulnerability.

---

## Detecting Cached Responses

During testing, it's crucial that you're able to identify cached responses. To do so, look at response headers and response times:

- **`X-Cache: hit`** - The response was served from the cache.
- **`X-Cache: miss`** - The cache did not contain a response for the request's key, so it was fetched from the origin server. In most cases, the response is then cached. To confirm this, send the request again to see whether the value updates to `hit`.
- **`X-Cache: refresh`** - The cached content was outdated and needed to be refreshed or revalidated.

The `Cache-Control` header may include a directive that indicates caching, like `public` with a `max-age` higher than `0`. Note that this only suggests that the resource is cacheable.

If you notice a big difference in response time for the same request, this may also indicate that the faster response is served from the cache.

---

## Exploiting Path Mapping Discrepancies

1. If there is a `/my-account` path, check if the response has an `X-Cache` header.

2. If there is no `X-Cache` header, try to add an arbitrary file path like `/my-account/abc.js`, `/my-account/abc.css` or try with different types of extensions according to cache rules.

3. If the response contains an `X-Cache` header and the same response, that means the web application is vulnerable to a web cache deception attack.

4. To confirm, access the `/my-account/abc.js` URL path from an incognito tab to verify that the path is cached on the server and can be accessed without logging into the account.

5. Craft a malicious URL and send the link to a victim. The victim's confidential information will be stored in the cache, allowing access later.

---

## Sample Exploit Code

```javascript
<script>
    // Redirect the victim's browser to the malicious URL
    document.location = "https://vulnerable.web/my-account/wcd.js";
</script>
