
CPU-Bound vs. IO-Bound :-
* CPU-Bound -> This means that the server’s brain is doing heavy mathematical work non-stop like encrypting passwords, compressing video or sorting millions of numbers. The processor is working on its maximum capacity.
* IO-Bound -> “IO” stands for Input/Output. This means that the server is doing nothing and sitting idle, waiting for the external services like database, a hard drive or external website’s API, to send information back over the network.

Concurrency vs. Parallelism :-
* Concurrency -> This is the Illusion of doing many tasks at once using just a CPU core. The CPU works on task A for milliseconds, hits pause, works for task B for milliseconds, hits pause, works for task A. It switches incredibly fast that it feels like everything is happening simultaneously, but the cpu is doing one single task at once at an exact moment.
* Parallelism -> This is actually doing the tasks side by side on the same exact literal time. To do this, you will need multi-core CPU because each core handles specific task and goes along from start to finish, running without ever pausing.

The Threading Model (The Heavy Way) :-
* The “Thread” is a dedicated workspace (A separate office for an employee) created by the server’s operating system (like linux). It was used in the Traditional way and even used today also by creating a brand-new, heavy workspace for every single incoming request.
    * The Problem :- Each workspace requires a large chunk of memory (RAM) just to exist. If thousands of user visits on your app, and the server makes the separate workspace for them it will completely run out of memory. Furthermore, the server wastes a lot of time and energy, just by saving and switching the context (a process called “context switching”) until it eventually crashes.

The Event Loop (The Single Thread Way) :-
* Instead of creating thousands of heavy workspaces, the server just uses one single thread (one workspace) to handle every single user.
    * How it works -> When the user 1 needs data from the database, the event loop says, “I will serve other users until the database replies me with data”. It instantly starts to serve user 2, user 3 or user 4. The exact microsecond database replies to user 1, the event loop picks that request backup and finishes it.
    * The Danger -> Because there is only one thread, we must never give it a complex math problem (CPU-Bound task). If the single thread gets stuck for serving only one user then it will not be able to spin the loop and every other user on the website gets completely frozen.
* Key Point -> Thread (dedicated workspace) & Event Loop (The one who performs tasks).

Virtual Threads (Goroutines) :-
* Instead of relying on the Operating System to create those massive, memory-hungry workspaces. The programming language itself (Like GO) manages the workspaces internally. It creates tiny, ultra-lightweight virtual threads. Because their memory requirement start at tiny 2KB and they are incredibly fast to switch between. you can run thousands of virtual threads at once without overwhelming the server.

Async/Await (The State Machine) :-
* Async is a keyword that tells the server that “this task involves waiting”, immediately return a claim ticket (promise) so the main program flow never freezes
* Await is also a keyword placed inside the async function that means “Pause this task here, and leave a bookmark so you know exactly from where to pick back up”, so the thread can serve other users until the real data finally arrives.
* Claim Ticket -> An empty placeholder that computer gives you right away, promising to swap it with real data second it’s ready.

Race Conditions & Locks :-
* Race Condition -> It is the condition when two or more user tries to read or update the exact same variable at the exact same milliseconds. Because they overwrite old data simultaneously, they do their maths and overwrite each other’s work resulting in losing one user’s updates permanently.
* Locks -> A safety mechanism that physically blocks the variable. If user A is updating the data, the user B is forced to wait in a queue until the user A is completely finished, saves the new data preventing overwrite of the data.  

 

