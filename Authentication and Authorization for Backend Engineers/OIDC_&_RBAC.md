OpenID Connect (OIDC) :-
* OpenID Connect is a Identity layer built on top of the OAuth 2.0, enabling application to authenticate and get profile information .
* OAuth 2.0 handles Authorization (What can you do?) and the OpenID adds Authentication (Who are you?) by using standardlized ID Tokens which provides user_id, name, email, issuer etc. 
* It solved one major problem suppose you get a dialog box of ‘Continue with Google’ in spotify to sign in, so now the google provides the user profile cantaining something like your name and your email. So in this the google provides Spotify your informations not your password.
* Single Sign-On (SSO) allows the user to authenticate once with a central identity provider and access multiple application without entering cresidentials again-and-again.

Role-Based Access Control (RBAC) :-
* Role-Based Access Control (RBAC) is an authorization strategy to assign permission to the user according to there defined role in the organization. It is less error prone because we don’t have to assign permissions individualy.
* RBAC for Role-Management, you analyze the requirements of the user to proceed his work further, so we assign them common roles based on their responsibilities.User-Role and Role-Permissions relationship helps us to assign the responsibilities efficiently.
* So in RBAC the flow will be :- Request -> Authentication -> Authorization(Role-Based Access Control), It will check if this user have permissions to do this work, According to it the Forbidden or successful response will be transmitted.

