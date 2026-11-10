The Core Problem: The Server Cannot Speak First :-
In traditional backend architecture, every interaction follows strict HTTP rules “The client initiates and the Server responds”. 
The server has no mechanism to start the conversation, the server is like a polite librarian it has the information but it only provides it when asked.

Evolution of Realtime Solutions :-
To solve this, we evolved 3 stages:
* Polling -> The browser runs in a continuous loop, asking the server every 3 seconds “Did anything change?”.
    * Why it’s bad -> It scales with the number of users, not with the number of events. If 10,000 users are online, the server will be hammered by 3,000 requests per second and 99% of the time, the answer is “NO”. It drains mobile battery by draining its phone’s radio and wastes the CPU just to say nothing happened most of the time.
* Long Polling -> The browser asks, “Did anything change?” but this time the server doesn’t reply immediately. It holds the HTTP request open (sometimes for a full minute) until an event(any new change that happens in backend) actually happens. Once the server replies, the connection closes and the browser initiates a new connection.
    * Why it’s bad -> There is a brief gap between receiving the response and opening the next connection, if any event takes place in during exact milliseconds gap. Then the user will miss the information or he will face heavy lag.
* Server-Sent Events (SSE) -> The browser makes an HTTP GET request and the server responds with a (text/event-stream) header then never closes the connection. The server keeps pushing data in this infinite pipe.
    * Pros -> The browser have native auto-connects, uses standard HTTP and uses (Last-Event-ID) to resume gracefully. 
    * Cons -> It is Uni-Directional means in this server can speak but the client cannot send anything in return.
* Websockets -> The ultimate solution. It creates a persistant, bi-directional connection between both client and server, they both can push messages to each other instantly over a single pipe.

Websockets: How it actually works :-
Websockets are not a magic; they sits on the top of the networking rules. Here is how they operate :-
* The Handshake -> It all start with HTTP GET request with an (Upgrade: websocket) header and a (Sec-WebSocket-Key. The server replies with (101 switching protocols) status. Once HTTP is dead, the raw bi-directional stream begins.
* Frames (Saving Bandwidth) -> Instead of heavy HTTP headers, websockets send the data in tiny “frames”. A small text message costs only 2-byte header.
* Masking (Security) -> Every frame sent by the client is XOR-masked with a random 4-byte key. This is not for encrypting your data; it was designed to prevent attackers from confusing internet proxies.
    * Cache Poisoning Attack -> It is a attack in which the attacker forces the browser to save a malicious page in cache instead of real-ones. 
* Heartbeats (Ping & Pong) -> TCP is blind. If a user walk into an elevator and loses mobile data, the TCP doesn’t tell the server. An idle and dead connection looks identical. To fix this the server sends PING frame, and the client must reply with PONG frame, if no PONG withing ~30sec then the server will kill the connection to free up RAM.

Hardware Limits: Can One Server Hold 1 Million Connections :-
If a server is holding idle websocket connection, what breaks first.
* Bottleneck 1 (File Descriptor: OS Limits): In linux, by default every network socket is a file, and it caps the open files by 1024. Your server will crash at exactly 1024 users if you don’t increase this limit. Languages like golang request +1M limits on startup automatically).
* Bottleneck 2: Port Exhaustion (Client-Side Limit): A TCP connection requires “4-Tuples” (Source IP, Source Port, Destination IP, Destinition Port). Suppose your testing computer is mobile phone, and its IP address is its SIM card. One SIM card has a limit of max 28,000 live calls (outgoing ports). So when you test the 50,000 connection test then your main server doesn’t crash instead your testing computer’s SIM calling limits gets exceed and it stops working.
    * The fix -> Give your computer new SIM card( IP address).
* Bottleneck 3: RAM (The Final Boss): In GO, holding a idle connection costs 9.7 KB of Heap RAM. If there are 1,000,000 of idle connection that will be roughly 10GB of RAM, which a 64GB server can handle with ease.

Multi-Server Scaling (The Stateful Trap) :-
Standard REST API’s are “Stateless” (Any server can Answer), but WebSockets are “Stateful” (Connection lives in the RAM of one specific server).
* The Trap: If you send some messages to the server user B in the server 2, the server 1 doesn’t know that the server 2 exist. So the user B will never receive the message.
* The Fix: Make something like a “Whatsapp group” between the 2 servers (Redis Publish/Subscribe or Kafka), so the server 2 will subscribe the group and take the message from the redis and deliever the messages to the user they are intended to be sent.
* The Danger (Fire or Forgot):  Redis Pub/Sub forgets the messages after sending it. If a server restarts for 3 seconds then the message sent by the redis will disappear permanently. If you want to be safe with this problem then use kafka that will store the messages in the disk-drive until the server doesn’t come online.

Production Nightmares: Fan-Out & Reconnect Storms :-
* Fan-Out Delay: Imagine there is a Discord group and 30,000 users are online and one user type “Hi”. Now your server have to send “Hi” to 30,000 different network sockets( used to send data over internet) and it can take 2 seconds to start the loop.
    * The Fix -> Distribute the work to the different background workers rather than setting the origin server to perform each and every task.
* Reconnect Storms: If the server restarts. Many user’s got disconnected, now when the server will start many users will ask for their data in seconds and query DB also for their data. Your database will blast. 
    * The Fix -> When your are restarting the server and it is starting now so cache the data in redis or CDN (Edge server), so that the traffic will not be able to reach to the DB.


