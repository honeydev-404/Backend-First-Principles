Session, JWT, Cookie :-
- We already know that HTTP is stateless it means server doesn’t remember previous request.

Stateful Authentication(Session) :-
* First of all what is session -> Session is a Server-Side Storage mechanism used to maintain state and track authenticated 
* Session Data with id is directly stored in the server-side.
* Session_Id is a Temprory Unique code that is used to recogize or track the user from every single visit on a website.
* There is also some expiry time on the session id.
* Redis and Database plays an important role when performing DB\REDIS lookup for session id.
* At the starting let’s understand with an example we put our credentials in sign in page then the user authenticates it then server creates a session payload and it stores it in-memory storeage like Redis etc, and then it provide a session id that is stored in form of cookie in the user’s browser.
* Then if we request any resource then the client’s browser provides cookie then the user get authenticated by the server by checking in the server if the session_id exists.
* There are some advantages of session is that it provides instant revocation(means- official cancellation,reversal of someting) 

Cookie :-
* So cookie is a small peice of data that the server instructs to store in the user’s browser it is primary used to maintain a state because “HTTP IS STATELESS”.
* Do you remember we have learned about one header above that is HttpOnly so its main function is to prevent the client-side Javascript from reading the cookies.
* Browser sends cookie in subsequent request because if we re-visit the and ask for some resource on the web the cookie will be sent to the server-side and we will get identified and permitted for our use of capabilities.
* As we visit a website first thing it asks for is to authorize yourself so if I have signed in the website then i leave it and after sometime I have to enter my login creds again so it will be a time-taking process. So the solution of this problem is cookie if I re-visit the website then it will identify me because the browser sends the cookie to the server, and the server uses the session ID stored in the cookie to find the user’s session in Redis.

JWT (Json Web Token) :-
* Json Web Token is a token format and it provides us Authentication Mechanism which contains a Digitally Signed approval for performing certain task and no need to re-provide any credentials after 1st time authentication.
* It also have some expiry time.
* It is hard to revoke (invalidate token) because it doesn’t include any server-side lookup if its signature is valid or it have been expired, Because here the server is blind it can’t check that who is using the JWT.
* To fix this revocation issue developers came with a solution :- They suggested us to make its expiry time shorter for about 5 mins, etc.
* There are 3 components of JWT ->
    1. Header - They typically contain metadata and consist of 2 parts :- Type of Algorithm used and The Type of Token.
    2. Payload - They contains the claims, Claims are information about the particular identity.
    3. Signature - Now comes the signature that whose signature is authentication us it have encoded header, encoded payload and a secret and then the algorithm in this specified header signs that.
* The output comes is three base64 url strings seperated with dots(.) like {Header.Payload.Signature} .

