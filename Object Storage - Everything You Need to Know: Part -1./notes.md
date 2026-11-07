
The Naive Design (Average Beginner Thinking) :-
The intuitive approach :-
* Browser send the data via multipart.
* Backend application extracts the file from the request.
* Saves it somewhere in the server’s local filesystem like (ext4).
* Stores the path or filename in the database so that it can be fetched by the user again and returned by the browser.
This works for the happy path on the single server with small files, but it in not production-ready.

Things to Know :-
* Docker -> A tool that packs all your dependencies that is being used by the project is known as Container Image, and when you run the container image it runs perfectly because it got exact same dependencies version as it used to get in the developers system. Remember its filesystem is ephemeral by default which means the data only lives until the container is alive.
* Kubernetes (K8s) -> It is an orchestration tool that automates deployment, scaling and management of containerized applications means helps in managing containers across multiple servers. If an pods goes down it creates a new one to keep the site online without a human manually doing this.
* Ephemeral -> Everything you write in the container’s filesystem exist only while that specific container is alive, the moment it is redeployed, restarted or killed all of its data is wiped off. Like a Docker container’s local disk is ephemeral.
* Durable -> The data still exist somewhere in the world, even if the machine dies, and we cannot recover it by waiting. Even if the machine dies, the data is still recoverable from the copies. 
* Availability -> The system is online and ready right now. If a machine dies user can still access the files because because the backup instances can take over the traffic.

Why Object Storage Exists :-
There are some problem with storing data in local-disk storage, the object storage comes as the solution for the constraints below.
* Problem 1 -> When a container restart its data get wiped out.
* Problem 2 -> If there are 3 instances and the files are stored in the instance 1, so the load balancer don’t know where to direct the request for the user’s wants. It also violates the core component of the horizontal scaling as it keep a file stateful (instance-specific data).
* Problem 3 -> If the disk gets filled don’t add more disks instead increase the size of existing one but also involves downtime and there must be max limit of disk capacity.
* Problem 4 -> If a server’s disk dies the user’s sole data is wiped of permanentely.
* Problem 5 -> If a user is using a 2g internet connection then the file transfer will be very slow and it will keep a server worker busy for just transfering bytes.
* Problem 6 -> If a file is written in DB and the insert fails so it will create and orphan file or if the row is inserted in DB and the file write fails then it will create some problems.

Three Kinds of Storage: block, file, object :-
* Block Storage -> It is a type of storage which provide raw, unformatter volumes like SSD and the data is broken into fixed-size, independent chunks with there address so called numbers. It is fast as it avoids file-system overhead and allows direct disk access.
* File Storage / Filesystem -> It is the abstraction above the blocks that adds directories, files, paths, permissions and POSIX semantics. It is very convenient for local use as it is used mainly in Operating systems, but the coordination required (Locks, atomicity or partial writes) doesn’t scale well across databases.
* Object Storage -> It start with opposite ques: What is the minimum interface that can scale to essentialy unlimited capacity? and they don’t have folders or sub-directories it is just illusion and to modify a file it must be completely overwritten The answer is four questions :-
    * PUT.
    * GET.
    * DELETE.
    * LIST.

Object storage: the interface and the trade-offs
Remember -> Object Storage is used when data is large and it has no structure of data.
* If you want to update a file you have to download it, modify it and then re-upload the new one leaving the concurrent-update problem far-away.
* It has no directory or sub-directory hierarchy to store the files.
* Every operation is a self contained HTTP request means any of the server can handle request.
* You never have to guess, resize or allocate hard drive partitions because the storage automatically expands forever as you upload.

What an Object actually is, the Folder Illusion, and Key Design :-
Object Storage Consists of :-
* Key -> It acts as the file’s ID or unique address. And the / is just a string.
* Value -> The actual file content (like video, picture or spreadsheet) in bytes. The term “Opaque Blob” means that the storage system is blind and it doesn’t care what type of data is stored.
* System Metadata -> Data about when the file was uploaded or deleted. like the file size, content-type or etag etc.
* User Metadata -> Extra, custom tags the developer choose to attach to the file for organization. Like uploaded-by or project-id.
The objects cannot just float freely; they must live inside a Bucket ( a top-level root folder / container ), creating a globally unique name combination so that the system can locate the exact files instantly without looking to the directory tree.

Folders are an illusion :-
* A key like { uploads/2026/cat.png } is just a one long string. The / doesn’t have no special meaning.
* In object storage, moving or Renaming a folder then copying all the files of the inside of the folder and then delete the previous one.
* When you click on the new folder without adding the file it cannot physically exist, to trick you the system creates an empty folder by creating a hidden, zero-byte black file ending with / so that it appears as a folder on the screen.
* Clicking on the folder doesn’t open any directory if you have a folder with a picture like { photos/cat.png } the browser chops the /cat.png part and shows it cleanly that lets us believe that we are opening a folder but we aren’t.
* The Zero Byte file does nothing but helps a folder to exist in a table like {myphotos/ }, and the dashboard recognizes the folder, hiddes the file and let it to be in exstance.

Designing the Key :-
* Never use the user’s raw file name as the key (it will create some security concerns).
* Instead, generate your own key and add random data like UUID in front of the key to prevent “hot partitions”( Bottlenecks cause when storing files with sequentially timestamps).
* Finally, embed the ownership in the key (TenantID/UserID/UUID) so the authorization checks can happen via the string comparison rather that hitting the database. Tenant id is a unique identifier for a specific group like (asml-corp).

The Two Planes & Eleven Nines :-
Object storage is split into two systems:

* The Data Plane -> Handles the raw bytes, instead of expensive replication( copying files in each and every instance ad backup ), it uses Erasure Coding. An algorithm that splits a file in 14 shards and generates 6 parity shards. These 20 shards is scattered across different systems and the system need only 14 to reconstruct the data. The math provieds “Eleven-Nines” (99.9999999%) durability, means you statically lose one file every 10,000 years.
* The Metadata Plane -> A distributed, strongly consistent B-Tree index that maps the key to the physical shard location. Because it is highly optimized for the direct lookups, asking it to scan ranges is slow like give me all the cars that have been parked here in 1 hr. You should track what files exist in your database rather than just listing the item in the bucket.

Conditional Writes (if-none-match) :-
Historically, object storage allowed silent overwrites. Now it support conditional overwrites, by passing a { if-none-match: * }header. By this the storage system will only save the file if the exact key doesn’t exist, if it exist then it will return 412 Precondition Failed error. This act as a distributed lock, preventing race condition where two users silently overwrited the other’s work.

Getting the File In: The Two Architectures :-
When the user uploads, you must decide how the data travels from the browser to the object storage bucket.

Architecture 1: Uploading Through Your Server (The Problematic Way) :-
The intuitive approach is to send the files to your backend API, which then forwards it to the object storage. However, this creates a severe bottlenecks -> 

* The Buffering Trap -> By default, the web fromworks read the entire incoming file into the server’s RAM. If 20 users upload 100MB files each, this will consume your 2GB of RAM. And it will run OOM and crash.
* The Streaming Fix -> To prevent the RAM crash, you can “stream” the file in which the server will read the file in chunks and pass them directly in the object bucket. Your RAM usage remains completely flat, even in 5GB file upload.
* The Ceiling you cannot Stream past -> Even with perfect streaming, you will hit hard network limit. If a user is on slow mobile network, the upload takes too long the load balancer will cut the connection. Furthermore, reverse proxies and API gateways have hard payload limits. They will simply reject large videos with a 413 payload too large error.

Architecture 2: Pre-signed URLs (The Production Standard) :-
* To avoid buffering trap and network limits entirely. You must remove the server from the path, let the browser upload the videos or files to the objects bucket directly.
* But your bucket is private you cannot give the browser your secret aws creds. The solution is Pre-Signed URL.
* Your backend takes the HTTP method, bucket name, the exact file key and the expiration time and hashes them with your secret key. It gives url to the browser, and now the browser can talk to you object bucket for few minutes without it knowing your secrets.
    * The Hole in the URL (POST Policies) -> A pre-signed PUT url doesn’t restrict the file size. To fix this, you don’t just sign a url; you sign a POST policy. This is a json document that tell the object storage to reject the request if the file is too large or the wrong content type.

The Two-Phase Flow (Syncing the Database) :-
If the browser uploads the file directly from the browser to the bucket, and your backend never sees it. So, how does the database knows that the file exists.
* Phase 1 (intent) -> The browser asks your backend to upload the file. The backend generates the secure, randomized object key, insert a row in the database with the status code pending, and returns the post policy in the browser.
* Phase 2 (completion) -> The browser uses the POST policy to upload the file directly to the storage bucket. Once completed, the browser calls a “completion API” in your backend.
* Verification -> The backend uses a lightweight HEAD request to the object storage. This doesn’t download the file; it just asks “Did the file got uploaded on the object storage in the same exact size”. If yes then it modifies the row from Pending -> Completed.
 
Cleanup and CORS (The Final Details) :-
* Cleanup (Dealing with abandoned uploads) -> User closes the tab or their internet drop mid-upload, this creates and orphan file. You must run a background worker (CRON jobs) that scans your database and completely remove from the database in next 24 hrs.
* CORS (Cross-Origin Resource Sharing) -> Because the browser is trying to send the data directly to a different domain instead of backend api, the browser’s security tools will block it. You must configure CORS policy and allow the PUT and POST operation in the bucket.







