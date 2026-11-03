
The Core Metrics: Latency and Throughput :-
* Latency (The Wait Time) -> This is the total time taken for a single request to travel from the user’s click, to the server, then to the database and then back to the user’s screen.
* Why Averages are useless -> If 99 requests take 50ms and 1 request takes 5s, the average looks completely healthy but one user faced a terrible experience of lag.
* Percentiles (P99, P95, P50) -> Engineers use percentiles to measure reality, if your P99 latency is 50ms it means that the 99% users faced 50ms latency, while those 1% user faced lag of 5 seconds. Engineers usually focus on fixing P99 and P95 latencies because these are the users that are doing valuable business logic or ones running complex queries (like processing a payment).
* Throughput -> This measures how many requests can be processed in a given timeframe (e.g., Request per second / RPS). Both Latency and Throughput are interconnected as the throughput goes up, the latency skyrockets when it reaches to the hardware limit and slows down.

Resource Utilization & The Traffic Jam :-
* Utilization -> This is the percentage of your system’s capacity that is currently in use.
* The Highway Analogy (Exponential Latency) -> If there is a highway that is at 50% capacity cars can switch lanes and move freely (low latency). At 80% cars slow down and at 100% nobody moves. Backend servers work exactly the same way. As the CPU utilization grows and reaches 100%, there will be Exponential (That doesn’t grow in straight line and grows faster and faster) Latency, as the request will be queued and wait for the resources to be free.
* The Golden Rule -> You can never run your system at 100% utilization. Real-world traffic comes in an unpredictable way “bursts”. You should keep some buffer (headroom) of 30-40% to absorb sudden spikes without crashing.

Finding Bottlenecks (Measure, Don’t Guess) :-
* The Problem -> When an API is slow, developers guess that it must be happening because of Database and then they waste weeks making it fast by adding caching. You must first measure where the actual time is being spent rather than guessing.
* Profiling (Flame Graphs) -> Profilers watch your code run and generate “flame graph” to show which function is eating up the CPU. However, profilers are best for CPU-bound task (Complex maths) and miss IO-Bound task (Like waiting for database and networks). 
* Distributed Tracing -> This attaches a tracker on the request and tells exactly how many milliseconds are spent where. Like (2ms in routing layer and 800ms in DB , this reveals the true bottlenecks).

Database Bottlenecks & Fixes :-
* The N+1 Query Problem -> Imagine fetching 20 blog posts (1 Query) and for each post extract their authors (20 Queries). You just made 21 network calls to load one page. The Fix :- Always use DB joins or bulk-fetching ORM features to grab all posts and their authors in just 2 queries.
* Indexes (The Library Catalog) -> If a library has no catalog, the librarian has to check every single book to find the one the user wants (e.g., Full table scan). An Index is a catalog (Usually a B-tree Structure) that keeps a sorted list making 4-second full table scan -> 40ms lookups.
* The Cost of Indexes -> Indexes take up storage space and most importantly they make the write operations slow as the update,insert or delete operation also has to be implemented on the indexes. Only index columns you frequently search by.
* Explain Analyze -> If you don’t know what to index, put EXPLAIN ANALYZE in front of raw SQL query. The database will show exactly from where it fetched the data and how, and tell if it was forced to do a slow full table scan.

 Connection Pooling :- 
* The Problem -> Opening a brand new TCP connection to a database for a single HTTP request is incredibly expensive. Furthermore, databases have hard limits ( e.g., max limit = 500). Without connection pooling, your server will open too many connections and crash the database.
* The Solution (Connection Pooling) -> A connection pool is a middleman that keeps the set of database connections permanently open. When your server wants to query something from the database, it borrows an open connection from the pool, runs query and then returns the connection to the pool for the next request to use.
* External Pooling (PgBouncer) -> It is a tool used to manage and share database connections across all small instances of application servers, preventing from database to get overwhelmed and crashing during high traffic spikes.

Caching Strategies :-
* Location :-
    * Local Cache -> Stored directly in the server’s RAM (Ultra-fast but get out of sync if you have multiple servers)
    * Distributed Cache -> Stored in the external services like Redis (consistent with all servers but add a bit of network latency)
* Cache Invalidation (The Hardest Problem) :-
    * Time-Based (TTL) -> The cache automatically deletes after the set time (e.g., 5 minutes). Hard to guess the optimal time (gets renewed in like 5s) , risk of showing outdated information.
    * Event-Based -> The code manually deletes the cached profile the exact millisecond when the user updates their profile in the database.
* Caching Patterns :-
    * Cache-Aside (most common) -> The app checks for the data in the cache if not there then it retrieve from the database and save it in cache then return it.
    * Write-Through -> Everytime when the data is updated, the app saves it in the database and in the cache at the same time before responding to the user.
    * Write-Behind -> When the data is being generated or updated, it is first stored in the cache memory so that user can access it and then it can take the data out from the cache and save into the database asynchronously.

Scaling (Vertical vs. Horizontal) :-
* Vertical Scaling -> Adding new and powerful specs and upgrading them in the same server. Like more CPU or more RAM.
    * Pros -> Incredibly simple and doesn’t require any architectural changes.
    * Cons -> Has a hardware limit as nothing can be added now. Creates a single point of failure as if one giant server fails it brings the whole application down as there is no distribution of work.
* Horizontal Scaling -> Keeping your medium-sized server but adding more side-by-side to distribute the workload.
    * Pros -> Infinite scaling potential. High redundancy(If one server crashes, others can take over), and it can be distributed geographically (like one in India or one in America).
    * Cons -> Highly complex, as we have to manage many machines not one and set up connections, external services or handle distributable state so the servers stay perfectly in sync.




