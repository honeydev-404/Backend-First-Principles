
Stateful Authentication :-
* Stateful Authentication is commonly used in many backend applications, especially for those which do not require scalability too much. In stateful authentication the reference ID (Session_ID) is saved in the client-side and the session data is stored in the server-side, now when the user requests something then the reference ID from the client-side is send to the backend application so that it can perform lookup for session data.

Advantages ->
1. Revoke the session anytime.
2. If we take one server for the session data then it is easy to manage for one-session-server scenerio.
Disadvantages ->
1. As long as the traffic increases on the web app then there will be need of more session server to handle everyone’s session data.
2. When the session-server increases it adds alot of complexicity as when the server increase then the request must be handled that in which server the request will go for session data.

Stateless Authentication :-
* Stateless Authentication is used to solve the disadvantages of the stateful authentication. In this type of authetication the digitally signed token is stored in the client-side and the server-side requires only the capabilities of verifying the digitally signed tokens.

Advantages ->
1. It doesn’t any overhead storing the session data in the server only the digitally signed token is send to the server-side and the server validaties that’s all the process require.
2. It is easy to scale as long as the server have the capabilites of validating the requests from the private key.
Disadvantages ->
1. It is hard to revoke as the server-side has no rights to delete any session.
2. The data can not be changed until the previous token is active and only allowed to change when the token is expired.
 
Where to use Stateless and Stateful :-
* Stateless ->
    1. Use stateless when you require high scalability and reduce server side session look-ups that leads to low latency.
    2. Use when you are integrating 3rd party services and hitting APIs in veryless amount of time interval that removes all the session authentication etc.

* Stateful ->
    1. Use Stateful authentication when your app requires High-Security like banking portals etc.
    2. Use it when you want instant revocation like you will be able to log out user instantly from your application and when you can do all the session look-ups in one server because of less traffic.

