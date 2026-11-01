The Core Problem & Concept :-
* The Problem -> Imagine a user is making some critical payment on the e-commerce site, if the server shuts down suddenly due to some reasons like code deployment the payment might get lost in the digital void or the user will get charged twice of the original price.
* The Solution (Graceful Shutdown) :- Graceful shutdown is just like teaching “good manners” to the server, when it is the time to shut down the server, it should not slam the door on its user. Instead it politely stop letting new users in and finishing the ongoing task of the users, cleans up mess and then closes the door.

How the OS Talks to the Server (Signals) :-
Every backend application runs as a “process” in an OS, when the OS wants to shut down the server it doesn’t just pull the power plug, it sends the messages called signals to the process. Your code needs to listen to these signals.
* SIGTERM (Signal Terminate) -> It basically says, “Excuse me, please finish what you are doing and then shut down”. Your app catches these signal and triggers its shutdown routine.
* SIGINT (Signal Interrupt) -> This is exact same as the SIGTERM but it is controlled/triggered by the user manually by the developer pressing (Ctrl + C) in the terminal. The backend should also handle this.
* SIGKILL (Signal Kill) -> It is the nuclear option used when the process is ignoring polite request like (SIGTERM and SIGINT) for too long, the OS wil eventually send SIGKILL and it will destroy the process instantly.

Step 1 of Shutdown: Connection Draining :-
When the server recieves the polite signal, the first phase it enters is called “Connection Draining”.
* The Restaurant Analogy -> If the server is closing for the night they don’t turn off the light while the people are eating. They don’t let new customers in and give time to the existing customers to finish there meal and leave.
* How It works in Code -> Your backend stops accepting new HTTP request. However, it keeps server just alive enough for processing the “in-flight” request (the user those who are downloading the file).
* The Timeout Limits -> You can’t wait forever. Server is programmed with strict timeout limits like 30 sec. The server will wait upto 30 sec for ongoing task to finish. If they are not done the by then, the server will force those processes to stop so that deployment can continue.

Step 2 of Shutdown: Resource Cleanup :-
Once all the user have finished their request and left, the server has to clean its workspace before turning it off.
* What needs cleaning -> If your backend opened connection to the DB or opened the file in the hard drive, it needs to explicitly tell those system that “I am Disconnecting now”. If it doesn’t, those open connections will sit there eating up memory until the system crashes.
* The Reverse Order Rule -> You must clean up resources in the reverse order of how you accquired them . For Example :- If you first opened connection to DB and then Redis then you must close Redis and second DB. This prevents error where one cleanup process accidently relies on a connection you already deleted.


