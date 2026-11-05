Statelessness: The key to Horizontal Scaling :-
* The Concept -> If you are running multiple backend servers (Horizontal Scaling), no single server can hold exclusive data (that is only saved in the specific server’s storage).
* The Threat -> If a user logs in server A and his session details get stored in its RAM, and then if the user’s request goes to second server then he will be blocked by the server as “unauthorized”.
* The Fix (Externalize everything) -> Never store the data on the server instance. Session must go to a centralized redis cache. Uploaded files must go to S3. Database must be external. Every server must be identical and replaceable  

Load Balancers & Routing Algorithms :-
* A Load Balancer (LB) is a traffic cop between server and the internet, decides which server will handle the next upcoming request.
    * Round Robin -> The LB deals with request like a deck of cards in a strict rotating manner (server A -> server B -> server C -> repeat From server A). Problem -> If server A gets complex database queries it might crash while the server next to it is sitting idle.
    * Least Connections -> The LB monitors the servers and sends the next request to the server with the fewest open connection(request that the server is currently working on). This naturally balances the heavy workloads among the servers.
    * Health Checks -> The LB constantly pings the server (e.g., every 1 second), expecting an HTTP 200 Success code. If a server crashes and gets 502, the LB will blacklist that server and reroutes the requests to the surviving servers until it recovers.

Database Scaling (Stateful Scaling) :-
* Scaling backend code is easy, scaling database(which remembers the data) is incredibly hard.
    * Read Replicas -> 70% of the web traffic is just reading data (GET request). You can create one “Primary” database to handle write/insert operations and multiple “Read Replicas” database globally for read operation.
    * The Replication Lag Problem -> Physics dictates that it takes time (e.g., 200ms) from the Primary database in US to replicate the new data in the Indian server. If a user updates their profile and refreshes immediately, they might hit replica before sync happens resulting in the user seeing the stale data (outdated information).
    * Sharding (Partitioning) -> If a table gets massive (e.g., billions of orders), you physically chop it in half. Orders from Jan-June in database A and orders from July-Dec lives on database B. This drastically cuts query latency but makes the infrastructure highly complex.

CDNs & Edge Computing :-
* The Speed of Light Cap -> Data travels through the fibre optic cable at a strict physical limit. A round trip from Tokyo to US server will always take at least 100ms , regardless of how optimized your code is.
* Content Delivery Network (CDNs) -> They fix this problem by placing the “Edge Nodes” right next to the users (e.g., node in Tokyo), they cache the static files like CSS, HTML, JS and Images and the latency goes from 100ms to 2ms. Cloudflare’s massive CDNs network easily absorbs DDoS attacks before they ever reach to the primary server.
* Edge Computing -> Instead of just serving the static files the Edge nodes can run tiny snippets of code. Example -> You can run JWT authentication on the Edge, if a token is invalid the Edge node instantly rejects the user in 2ms, saving your primary server’s resource from getting wasted.

Asynchronous Processing (Message Queues) :-
* The Concept -> Never make a user wait for the process that can happen in the background.
* The Fix -> When a user tries to login, the server must not let the user wait for 300ms for the email’s service provider to respond instead just authenticate the user and give the success message then let the user perform operations and queue the “welcome Email” as the background job because it can happen asynchronously. And when the worker pulls the task from the queue it will then send the email.
* Use Cases -> Video encoding, Image Resizing or sending notifications.

Microservices vs. Monoliths :-
* Monoliths -> All code lives in one codebase and runs as a single process. Extremely simple to debug, test and deploy. Stick to this until you are forced to change.
* Microservices -> Breaking the app into small services and they can also talk over the internet.
* Why use It -> It is a tool for scaling not just machines. If you have 200+ developers, the monoliths becomes bottleneck because they have to be dependent on other services. Microservices allows to the payment team to deploy their code independently without waiting for the notification team. It also allows us to mix tech stack like ( e.g., Node.js for API routing, Rust for heavy image processing ).
* The Cons -> Network latency, massive level debugging and nightmare-level data consistency issues.

Serverless Computing -> 
* Traditional Servers -> You rent a specific machine (e.g., 48gb RAM) 24/7. Predicting the traffic is impossible, if you guess too low the server crashes, if you guess too high you end up paying thousands of dollars.
* Serverless (AWS Lambda) -> You provide the code, and the cloud dynamically spins up a tiny virutal machine the exact millisecond when the request arrives. You are billed only for the execution time.
* The Cold Start -> Because there is no server permanently running so the first request forces the provider to boot up the OS and language runtime. This causes a noticeable delay “cold Start”  on the first request.



