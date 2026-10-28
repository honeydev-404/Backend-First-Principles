
The 5 Types of Backend Errors :-
* Logic Errors:- The most dangerous. The app doesn’t crash but it starts to perform unexpected operation like (applying a discount twice or doubling the price of a product). Cause: flaw of program’s algorithm, design or implementation.
* Database Errors:-
    * Constraint Violations Errors-> These errors occur when application tries to write or modify data in a way that breaks the db’s structural rules.
    * Connection Errors -> These occur when application is not be able to successfully communicate to the database mangement systems (DBMS). Example -> DB is overloaded.
    * Query / Deadlocks -> It is a gridlock where two or more processes get stuck because one has something that others are waiting for to operate further.
* External Servive Errors :- Third-parties API’s ( Stripe or AWS S3 ) failing due to network partitions, rate limiting, timeouts , or completely Service Outages.
* Input Validation Errors :- The first line of defence. User sending data that fails format, range or required fields. Usually result in (400 BAD REQUEST).
* Configuration Errors :- Missing Environmental Variables. Best Practise -> Validate all .env valiables at the server startup and crash the appilication immidiately if some files are missing rather than crashing it in runtime.

Proactive Preventions (Health Checks) :-
* Remember :- The best error handling happens before the error.
* HTTP Status Checks :- Expose a /health endpoint that return a 200 OK or a tiny JSON payload. If it fails, the system stops sending traffic to the broken instance.
* Deep Database Check -> Don’t just check if the DB is connected, run a light representative query like ( SELECT 1; ) to verify response time and data integrity.
* External Service Checks :-  Run test transaction ( .e.g., strip test mode ) or generate test auth token periodically to ensure that the external service is actually responding.

Error Response Strategies :-
* Recoverable Errors (e.g., Network Connections) :- Use Exponential Backoff & Retries (wait 1 min -> 2 mins -> 4 mins). 
    * RULE -> Don’t use retries it will overwhelm an already stressed system.
* Non-Recoverable Errors (e.g., DB is completely Down) :- Use Containment & Graceful Degradation (means you application lowers its quality and features but keeps it online). Think of it like it is in “Survival Mode”.
    * Switch to the cached data it will be old but the pages still loads perfectly.
    * Disabling the non-essential features like “you might also like section”.
    * Provides Fallbacks (Containing the Blast Radius) like instead of ugly crash screen you serve the user-friendly fallbacks.
* Error Boundries (Microservices) :- Use message queues (RabbitMQ) to decouple services so that a fatal error in one service doesn’t crash an entire system. The work should be distributed in entirely separate miniature application.

Global Error Handling (The Final Safety Net) :-
* Instead of writing the error messages in every database function, bubble all errors up to a single Global Error Middleware.
* How it works :- Handler, service and repository throws or returns error upward. The Global Middleware Catches them, identifies the type and frames the HTTP response.
* Example 1 (Validation) :- Middleware sees a validation error -> Returns 400 Bad Request and highlighting the field with error.
* Example 2 (Foreign Key) :- DB sees a “No Row / Invalid Foreign Key” error -> Returns 404 Not Found.
* Why it’s essential :- Reduces code redundancy and gurantees that no error slips through to become unhandles 500 Internal Server Error.

Security & Error Handling (Red Teamer Focus) :-
* Error handling is the most common vectors of security breaches. 
* Never Leak Internal Details -> If a DB throws a constraint error, NEVER send raw SQL messages to the client. It leaks internal structure details of the DB such as Tables name or column constaints leading to SQL injections.
    * FIX -> Catch the error in the Global Middleware and return a generic response such as “Something went wrong”.
* Authentication Ambiguity :- Do not tell the user “Email not found” or “Incorrect password”.
    * The Exploit -> Attacker uses “Email not found” to perform user-enumeration attacks, mapping out valid account before brute-forcing a password.
    * The Solution -> Always use Generic messages such as “Invalid Email or Password” this gives no context to the attackers.
* Log Sanitization :- Never save logs that contains bank details, passwords or API keys. If a company gets breached then the clearest secrets are exposed to the world. Log the User_ID and a Correlation_ID instead. 


