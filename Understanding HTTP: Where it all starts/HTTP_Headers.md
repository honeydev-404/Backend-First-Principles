HTTP Headers :-
Question - What are HTTP Headers.
Ans - Headers are the metadata about the request/response.
Example - They are like envolope of the header Which contains additional informations.

Request Headers -
* Content-Type: application/json  <What format my data is in>
* Authorization: Bearer token123  <Proof of authentication>
* Accept: application/json  <Which format I can handle>
* User-Agent: Brave/1.0   <What browser or client I am>
* Cookie: session=xyz123  <Here’s my session data>

Response Headers - 
* Content-Type: application/json  <Format of my response>
* Cache-Control: max-age=3600  <Your can cache it for 1 hour>
* Set-Cookie: session=xyz789  <Save this cookie in the client side>
* Access-Control-Allow-Origin: *  <CORS - Who call this api>
 
Security Headers - 
* Strict-Transport-Security (HSTS): Forces the browser to use HTTPS.
* Content-Security-Policy: Controls which resource/content the browser is allowed to process.
* X-Frame-Options: Controls if a webpage can be loaded inside the current webpage/iframe.
* X-Content-Type-Options: It prevent MIME-Type Sniffing. {MIME stands for Multipurpose Internet Mail Extension was introduced to extend the format of the simple plain text to include some multimedia content such as images, video, and attachement.}
* HttpOnly- It prevents Javascript apis from accessing cookies.
* Secure- Makes the cookie sent only over HTTPS.
