## **Detecting Cached Responses**

Add a cache buster parameter ```?cb=1``` in the request. Change the number in the parameter before new request, so that we don't get a cached response. 
<br>
<br>
To confirm caching behavior, inspect response headers and time differences:

- **`X-Cache: hit`** – Response served from the cache.  
- **`X-Cache: miss`** – Response fetched from the origin server and potentially cached.  
- **`X-Cache: refresh`** – Cache was outdated and revalidated.

Additionally, check `Cache-Control` headers for caching directives like `public` with a non-zero `max-age`.

---

### Web cache poisoning with an unkeyed header

Some websites use unkeyed headers to dynamically generate URLs for importing resources, such as externally hosted JavaScript files. In this case, if an attacker changes the value of the appropriate header to a domain that they control, they could potentially manipulate the URL to point to their own malicious JavaScript file instead. We can use ```X-Forwarded-For:``` header for this.

**Step**
```
  1. Add a cache buster parameter ?cb=1 in the request.
  2. Add X-forwarded-For header in request.
  3. Add a arbitrary domain (example.com) and send the request.
  4. If the example.com reflected in response page, we can now place our malicious link in it.
```
The malicius link contain with js file will trigger on all users browser if the site is vulnerable to the Web Cache Poisoning.
     
---

### Web cache poisoning with an unkeyed cookie

Cookies are often used to dynamically generate content in a response. If the response of the request is cached, then all subsequent users who tried to access the poisoned page will get the malicious content. 

**Step**
```
  1. Add a cache buster parameter ?cb=1 in the request.
  2. Identify if any cookie value reflecting in the response.
  3. If the cookie value reflected in response page, we can now place our malicious javascript code in it.
```

---

### Web cache poisoning with multiple headers

Identify any unkeyed header using param miner and craft a attck.
<br>
<br>
[Poc: Web cache poisoning with multiple headers](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers)
