
Authentication Security :-
* When you build a sign in application window then there are two fields Username and Password.
* So from the normal human’s perspective he will say that if the username is wrong then then  message will be ‘Username is incorrect’ and is password is wrong then ‘Password is incorrect’.
* So in the above lines if we observe something as Backend learners is that in the above line if one field will be correct than the Hacker can exploit the fields by user/password enumeration and steal the cresidentials.
* So we should write somethings safe as ‘Authentication Failed’ with this method there will be some complexcity for the hacker to exploit the fields.
* We as backend developers must keeps pushing our wondering capacity and we must think from the perspective of an attacker, that wil help us build safe, secure and more robust systems.

Password Hashing :-
* Hashing is the process of converting your plain text password into untelligible form. This process happens through Mathematical algorithm called Hash Function.
* Plain text password -> fixed-length string of characters.
* Hashing Flow :- You sign in a website -> You create a password -> Now the hash function is called -> It will converts your plain text password into a Hashed value -> Now it will stored in the database.
* Common hashing algorithims are :- bcrypt, Argon2 and scrypt.

Password Salting :-
* Password Salting is the solution of one major problem that if a user creates a password that is identical to the other user than obviously it will create same hash then these password will be vulnerable to the Rainbow-Table Attack (Pre-computed table of password hashes.) and attacker can attack multiple accounts at once.
* So the Solution is to add random string (The Salt) in the beginning of the passwords and then hash them.
* Suppose :- Password (Hashing@123) -> so in the starting of this some salt will be added so it will look like -> Bw5BWy8Hashing@123.

Timing Attack :-
* Side-Channel Attack -> It is a type of attack when an attacker tries to extract some cresidentials by physical properties such as power consumption etc.
* Timing Attack -> It is a side-channel threat where an attacker measures how long various computer system takes to process certain task and try to steal cryptographic keys or passwords. 


