Configuration Management (The Core Concept) :-
* What it is :- It is the “DNA” of your application, it controls how the code works across different environments without changing the codebase.
* The Misconception :- It is not just about hiding database passwords. It controls how feature toggles, timeouts , rate limiting and bussiness rules.

The 5 Types of Configurations :-
* Application Settings -> These are the basic rules of how your server operates, Like Port numbers, API timeouts limits, log levels, and DB connection pool size.
* Database Configs -> This acts as the map or key for you database. To fetch or save data, your backend must be configured with the exact URL, Port, Usernames, Passwords, and DB Name.
* External Services -> Allows the app to connect to the external services such as (Resend -> Email) , (Stripe -> Payments) and (Clerk -> Auth). It acts as the app’s ID to prove its permission to those outside tools.
* Feature Flags -> Toggle used to dynamically turn new features on/off. It let’s some resources to be accessed by the user for testing purpose.
* Bussiness Rules & Security -> Sets the strict law for the app, such as session timeout and maximum order spending limits. These config mainly control platform’s logic and security boundaries.

Storage Mechanisms (Where Config Lives) :-
* Environment Variables (.env) -> The standard for the basic apps and stored in the plain text file called .env . Libraries can read the files and inject the values in OS environment.
* Configuration Files ->Complex apps often need more structure than a simple text list. Commonly written in YAML or TOML and stores configurations. Common in open source projects like (config.yaml).
* Cloud Secret Managers -> Enterprise-grade storage. For enterprise-level apps, keeping passwords in simple text files is too dangerous. Companies use tools like hashicorp vault that encrypt the password while they are stored and deliever them to the backend when it boots up.
    * Advantage :- Automatically handles encrypting the secrets both “at rest” (in the database) and “in transit” (while being fetched by your app).

Environment Priorities (Why configs change) :-
* Configuration values changes based on where the app is running because each environments have entirely different goals.
    * Devlopment (Local) -> The main priority of the developer is speed and debugging. Therefore, logs are set to debug for detailed logs and db connection pool is set too moderate, So that the developer can test feature rapidly.
    * Staging -> The main priority is to mirror the production based servers with minimizing cloud cost. Like logs at test mode and connection pool to 2 because to save money as there is no real traffic.
    * Production -> The main priority is extreme reliablity, security, and handling high user traffic. Logs are kept at info and connection pool size to 50 so that many thousands of users can be handled.

Security & Best Practises (The Golden Rules) :-
* Never Hardcode Secrets -> Never write API keys or passwords in Source code.
* Least Privilege Access -> Restrict who can see what. Frontend developer do not need to access DB credentials, or Backend developer do not need to access AWS infrastructure.
* Rotate Secrets -> Periodically generate API keys or passwords as the may be compromised somewhere.
* The #1 Rule (Validate on startup) -> Validate the schemas through the libraries like Zod in Typescript to check configuration files before the server finished booting.



