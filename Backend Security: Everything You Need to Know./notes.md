
The Security Mindset & The Core Problem :-
* The Root Cause -> Almost every security vulnerablity starts when the developer makes an assumption (like developer thought that the user’s input will be clean). Attackers tries to find these assumptions and exploit the systems.
* The Boundary Rule -> Vulnerablities arises when data crosses from one system to another like (Browser talking to the Server or Server talking to the Database).we must always ask ourself, if our system is treating the user’s data/input as executable code.

Injection Attacks (SQL & COMMANDS) :-
* The Injections -> Injections happens when your system gets confused and starts to treat user provided data or user input as executable programming commands.
* SQL Injection -> An attacker types special characters ( Single Quote ‘ or Hyphens —) into login field, If your application uses string concatenation to build SQL queries then your database will treat them as executable commands. This allows the user to bypass login fields (OR 1=1) or delete the entire table (DROP TABLE). Remember: Never use string concatenation.
* Command Injections -> This is similar to the SQL injection but in this the attacker injects the operating system commands like ( rm -rf / ) into input field that will interact with the server’s terminal and execute these commands.
* The Fix -> NEVER build queries or commands with string concatenation. Always use parameterized queries (or Prepared Statement) for user input. This forces the data to treat the user’s input strictly as the raw text data not executable commands. Use placeholders like (?).

Authentication & Password Storage :-
* The Threat -> If your store password in plain text then if attacker gains access of your database then he can enter the user’s account acrossss  the network.
* Hashing is not enough -> Hashing is the one-way math function that turns password into gibberish. However, attacker can use “Rainbow Table” (List of pre-computed hashes) to reverse-engineer basic hashes.
* The Fix (Salting) -> Add a “salt” (a randomly generated string unique to each and every user) to the password before hashing it. This create rainbow table useless because every hash becomes entirely unique.
* Use Slow Algorithms -> Modern graphic cards can guess billions of passwords per second. You must use purpose-build “slow” algorithms ( like bcrypt or Argon2id ) that are equipped with “cost factor”. This intentionally slows down the maths and making offline brute-force gussing take centuries instead of days.
* Rate Limiting -> You must limit how many times someone can attempt log-in with a specific timeframe. Use layered rate-limiting (like per-ip limits, per-account limit or global limits) to block automated hacking bots.

Sessions & Tokens (Stateful vs. Stateless) :-
* Stateful Sessions (Recommended) -> The server generates a massive, unguessable random string (128-256 character) called a session ID, stores the session details in the database and sends just the ID to the user.
* Stateless (JWT) -> The server puts all the session data to the token that is kept and stored by the client’s browser, and it gets signed cryptographically and stores nothing in the database.
* The JWT Problem -> Because the server doesn’t store anything, you cannot easily revoke (to formally cancel something) a JWT or log user out immediately if account gets compromised.
* The JWT Fix -> If you strictly decided to use JWT then give the access token a very short lifespan (like: 5 mins) and use a refresh token to constantly use new one.
* Cookie Security Flags (CRUCIAL) :-
    * HttpOnly :- Prevent malicious javascript to read you auth cookie.
    * Secure :- Ensures that the Cookie is only sent on the HTTPS connection so it can’t be stolen on Public wifi.
    * SameSite :- It is a cookie rule that tells the browser when it is allowed to send the cookies along with the request coming from other websites. Main job is to prevent Cross-Site Request Forgery (CSRF).

Authorization (BOLA & BFLA) :-
* BOLA (Broken Object Level Authorization) -> Also known as IDOR and ranked as the #1 vulnerability. This happens when the user asks for something that doesn’t belong to him if the server returns the result of someone then it has BOLA security flaw and if it doesn’t then it is not vulnerable to BOLA flaw. Example -> User 12 tries to access the receipts of User 134.
    * The BOLA Fix -> The backend must never trust an Object_ID that is provided by the client without verifying the ownership and permissions first.
    * Do not leak info -> If the user is requesting an ID they don’t own, return a 404 NOT Found rather than 403 Forbidden. A 403 confirms that the item exists, which will help attacker to map your system for future attacks.
* BFLA (Broken Function Level Authorization) -> A regular user manages to access admin only URL because the developer assumed that it will be secret because nobody knows it exists.
    * The BFLA Fix -> Always check the explicit user’s role (role == admin) at the routing layer to before giving access to the sensitive funcitons.
    * Default Deny -> Write the code so that is the action or route is not explicitly permitted, it is automatically denied.

XSS & Misconfigurations :-
* XSS (Cross-Site Scripting) -> An attacker tricks your site into saving malicious Javascript (like hiding it down in a markdown blog comment). When other users views that comment, the scripts runs in their browser and steals session cookies.
* The XSS Fix -> Sanitize all the user-provided HTML before saving it in the database. Use Content Security Policy (CSP) header to block the unapproved scripts from the list.
* Secret Management -> Never put Passwords, API keys or JWT secrets in the source code. Keep them in environment variables (.env) or cloud vaults. If the key get pushed in the github accidently then remove it immediately and rotate it (generate a new one).
* Production Logs -> Never leave your server logs in Debug mode in production mode as it gives extra details about the queries and if the attackers get the access of the logs he can compromise the server, use Info mode in production. 



