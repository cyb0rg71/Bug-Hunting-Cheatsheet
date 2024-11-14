## **Cache Rules**

Attackers exploit cache rules applied by web servers. Below are common rules that make websites vulnerable to web cache deception:

### **1. Exploiting Static File Extension Rules**

#### **Description**:
Some caching mechanisms rely on file extensions like `.css`, `.js`, or `.jpg` to categorize content as static.
###path mapping discrepancies

**step**

1. Add ```/wcd``` an arbitrary path after original path
2. If the the response is same, now add an file extension like ```.js``` or ```.css```
3. Now make an exploit using the crafted url

### path delimiter discrepancies

**step**

1. Add a delimeter ```;``` after original path
2. If the response is same, now add ```/wcd``` path with extension.
3. Now make an exploit using the crafted url

### path delimiter decoding discrepancies

**step**

1. Same as delimiter discrepancies but decode ```;``` into ```%3b```

---

### **2. Exploiting Static Directory Rules**

#### **Description**:
Static directories (e.g., `/static/`, `/assets/`) often cache content unconditionally.

#### **Steps**:
1. Append/prepend static directory prefixes to dynamic endpoints (e.g., `/static/user/profile`).  
2. Request while authenticated to cache sensitive information.  
3. Access the crafted URL without authentication to retrieve cached sensitive data.

---

### **3. Exploiting File Name Rules**

#### **Description**:
Specific filenames (e.g., `robots.txt`, `favicon.ico`) are considered universally static, creating opportunities for exploitation.

#### **Steps**:
1. Request sensitive content with an appended filename (e.g., `/user/profile/robots.txt`).  
2. Sensitive data is cached as the static file.  
3. Access `/user/profile/robots.txt` to retrieve cached sensitive information.

---

### **Combined Exploits**  
Attackers often mix rules for broader vulnerabilities:
- Example: `/static/user/profile/favicon.ico` or `/assets/user/profile.jpg`.

---

## **Real-World Example**

A banking app with user-specific dashboards (`/dashboard/`) and caching rules for:
- Static file extensions (`.css`, `.js`).
- Static directories (`/assets/`, `/static/`).
- Specific filenames (`favicon.ico`).

An attacker exploits these rules to cache sensitive account data, enabling unauthorized access.

---

## **Constructing a Web Cache Deception Attack**

### **Steps**:
1. Identify sensitive dynamic endpoints using tools like Burp Suite.  
2. Detect discrepancies in how URLs are processed by the cache and origin server.  
3. Create malicious URLs leveraging these discrepancies (e.g., `/my-account/.css`).  
4. Induce victims to visit the malicious URL. Their response is cached.  
5. Retrieve cached data by accessing the same URL later.

---

## **Detecting Cached Responses**

Check response headers and time differences to confirm caching:  
- **`X-Cache: hit`** – Response served from the cache.  
- **`X-Cache: miss`** – Response fetched from the origin server and potentially cached.  
- **`X-Cache: refresh`** – Cache was outdated and revalidated.  

Additionally, analyze `Cache-Control` headers for directives like `public` with non-zero `max-age`.  

---

## Exploiting Static File Extension Rules

### **Exploiting Path Mapping Discrepancies**

### **Steps**:
1. Test for `X-Cache` headers on paths like `/my-account`.  
2. Append file extensions (`.css`, `.js`) or static directory prefixes (`/static/`) to these paths.  
3. If `X-Cache: hit` is present, confirm vulnerability by accessing the modified URL in incognito mode.  
4. Craft a malicious URL to cache victim responses.  


### **Exploiting Path delimiters Discrepancies**

### **Steps**:
1. Test for `X-Cache` headers on paths like `/my-account`.  
2. Append file extensions (`.css`, `.js`) or static directory prefixes (`/static/`) to these paths.  
3. If `X-Cache: hit` is present, confirm vulnerability by accessing the modified URL in incognito mode.  
4. Craft a malicious URL to cache victim responses.


## **Sample Exploit Code**

```javascript
<script>
    // Redirect victim to the malicious URL to cache their response
    document.location = "https://vulnerable.web/my-account/wcd.js";
</script>
```
---
