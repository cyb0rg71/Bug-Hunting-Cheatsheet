## **Detecting Cached Responses**

Check response headers and time differences to confirm caching:  
- **`X-Cache: hit`** – Response served from the cache.  
- **`X-Cache: miss`** – Response fetched from the origin server and potentially cached.  
- **`X-Cache: refresh`** – Cache was outdated and revalidated.  

Additionally, analyze `Cache-Control` headers for directives like `public` with non-zero `max-age`.

## **Cache Rules**

Attackers exploit cache rules applied by web servers. Below are common rules that make websites vulnerable to web cache deception:

### **1. Exploiting Static File Extension Rules**

#### **Description**:

Some caching mechanisms rely on file extensions like `.css`, `.js`, or `.jpg` to categorize content as static.

#### path mapping discrepancies

**step**

1. Add ```/wcd``` an arbitrary path after original path
2. If the the response is same, now add an file extension like ```.js``` or ```.css```
3. Now make an exploit using the crafted url

#### path delimiter discrepancies

**step**

1. Add a delimeter ```;``` after original path
2. If the response is same, now add ```/wcd``` path with extension.
3. Now make an exploit using the crafted url

#### path delimiter decoding discrepancies

**step**

1. Same as delimiter discrepancies but decode ```;``` into ```%3b```


### Web cache deception lab delimiter list

You can use the following list to help you solve the Web cache deception labs.


```txt
!
"
#
$
%
&
'
(
)
*
+
,
-
.
/
:
;
<
=
>
?
@
[
\
]
^
_
`
{
|
}
~
%00
%21
%22
%23
%24
%25
%26
%27
%28
%29
%2A
%2B
%2C
%2D
%2E
%2F
%3A
%3B
%3C
%3D
%3E
%3F
%40
%5B
%5C
%5D
%5E
%5F
%60
%7B
%7C
%7D
%7E
```

### **2. Exploiting Static Directory Rules**

