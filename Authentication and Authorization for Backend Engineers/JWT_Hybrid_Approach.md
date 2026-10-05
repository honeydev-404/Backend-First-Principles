JWT Hybrid Approach :-
* First of all the JWT Hybrid Approach is authentication pattern which combines fast, statelessness and performance JWT with a secure control over the server-side by adding a Block JWT lookup so no one can use the blocked or un-authorized JWT for retriving data.
* So the flow goes like first the JWT is sent by the client then the server takes it and then search in the blocked JWT list server if not found then the JWT will be authenticated and if found then there will be some error.
* But it has a drawback that it also needs a server to be added for recording blocked JWTs.
