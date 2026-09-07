What is the meaning of Stateless and Stateful :-
* Stateless here means that the server doesn’t have remember any of the previous interactions with the client and each an every request is treated as a new one.
* Stateful is just the opposite of the statless because it remembers the previous requests and interactions. 
* Each request must contain the information the server needs to process that request.
* Yes, HTTP itself is Stateless and doesn’t remember previous request or interactions.
* Benefits :- Scalability, Simplicity and it is also storage efficient because it does not store the previous intreaction informations.
 
How to maintain the State :-
There are many ways to make the server stateful and here I introduce you to three new concepts -
 
-Cookie-
* A Cookie is a small text file that is created by the web browser and stored on a client-side storage mechanism (browser). It allows the website to remember stateful information such as login cresidentials, shoping cart contents and user preferences etc. 
* Cookie are Primarly used for Session Management, Personalization and Tracking.
* Revocation is Easy.
* It is mainly stored in the browser’s cache.
* It is a Client-Side Storage.

-Sessions-
* The Sessions are the server-side storage mechanism that stores the uses’s state and authentication in HTTP requests, solving the problem of stateless mechanisms and storing cookies. Sessions are Stateful obviously.
* Sessions are the mechanism that makes the server "remember" who you are.
* The Session_Id is stored in a cookie.
* They can use tools like Redis that is an in-memory mechanism that lets us to store the data or information in the ram not the persistant memory like disk etc.
* Redis is famous for its Fast Storage Lookups.
* It is a Server-Side Storage.

-Tokens-
* A token is a piece of information/credential that the client sends with requests so the server can identify or authenticate the client.
* The token can be sent in a request, commonly through the Authorization header.
* Unlike server-side sessions, the server can use the token to determine/authenticate the request without necessarily storing the user's entire session state.
* Tokens are commonly used for authentication and authorization.

