What is OAuth :-
* OAuth stands for Open Authorization, it is a standard of giving some access to some application without directly giving away the password.
* Lets take an example suppose you have an Facebook account now the ESPN.com want some of your data for some reason so to authorize that you can give you password directly bad idea if some data breaches happen in ESPN then you password can be used on your behalf.
* The solution for the problem above is to have OAuth so we can give authorization to some services without giving away out password.

Components of OAuth flow :-
1. User/Resource Owner -> The user is the one who starts everything, he is the one who demands something like in signing in window.
2. Client -> The client is the one who sits between User and Authorization & Resource Server and also exchange the Token/Codes between authorization and resource server and talks to client directly.
3. Authorization Server / Token Service -> the Authorization server issues token/code that will be sent to the resource server to identify/find the resource from the code number and then sends the code to the client.
4. Resource Server / Stores Data -> Now the client sends the Auth code to the resource server then returns the access tokes and it’s main works is to serve protedted data to the client and at last the client sends the access token to the user and now the whole flow starts from user and ends to the user only.

OAuth 1.0 :-
* In OAuth 1.0 was also one of the way of sharing access to some service without directly giving away passwords, it particially failed because there were some steps which included very heavy cryptograpic signatures and it was very time taking.
* During the time when the organizations were using OAuth 1.0 API’s, they were facing some challenges,complexcity and need of improvement.
* For solving this problem the complete rewrite of OAuth 1.0 came as OAuth 2.0 .

OAuth 2.0 :-
* OAuth 2.0 came as solution of the problems of OAuth 1.0 as it included some cryptographic signatures so in this they decreased the use of expensive computation as giving access by HTTP headers and bearer token.
* So first the user tries to sign in google so I am the {user/resource owner} and the app like google where I am trying to sign in is client which will call another external service then it will request the authorization server and the it will return authorization code remember authorization server issues authorization codes/access tokens , so now the client will send the authrization code to the authorization server and then it will send the access token to the client and client will give it to resource server and then the user will login.

OAuth 1.0 vs OAuth 2.0 :-

It was mainly focused on web-application interaction.	It was focused on both web and non-web application intercations making better authorizations flow.
It needs to generate signatures on every API calls.	It doesn’t need to generate signatures on every API call because It uses TLS/SSL for communication.
In OAuth 1.0 the access token can be used for years.	In OAuth 2.0 the access tokens have short expiration time, reducing the chances of illegal actvities.
