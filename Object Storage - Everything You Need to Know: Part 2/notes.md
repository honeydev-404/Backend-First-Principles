Why a Single PUT Stops Working :-
Uploading files in one go (a Single PUT Request) works with the small file but it breaks when there is some massive files due to certain reasons :-
* The 5GB Hard Limit -> Cloud Provider physically reject any single upload exceeding 5GB, as if it fails it cannot be resumed and will waste a lot of time.
* All-or-Nothing -> If a user uploads a 5GB file and it is at 4.8GB and the WIFI drops for a second then the entire file upload fails. There is no “resume” button in PUT that will waste alot of time, they have to start from scratch.
* Throughput Limits -> A single TCP connection gets bottlenecked as it will decrease the packets transfer rate due to minor packet drops.
* User Experience -> The user cannot pause or resume the upload, and the progress bar will be inaccurate due to network speed.

The Solution: Multipart Uploads :-
To handle huge files upload break them into chunks using multipart upload. This involves exactly 3 API calls :-
* Create Multipart Upload -> The server tells the bucket to be prepared and make some space (staging area) for the files in uploading process. The Upload ID helps in tracking the chunks of file, from where to resume and also in identification.
* Upload Parts -> The browser upload the big file in small chunks (e.g, 16MB) directly to the bucket in the parallel(means other chunks are also beign uploaded sideways). If chunk #7 fails then only #7 chunk will be retried, each successful chunk returns and Etag(a digital receipt that holds information about the chunk for many operations).
* Complete Multipart Upload -> A server sends the Upload ID and ordered list of all Etags to the bucket. The bucket stitches them together logically with the help of etags and takes milliseconds even for 50GB file.

Choosing the Part Size & The 10,000 Cap :-
* The 10,000 Cap -> Object storage has a strict limit of 10,000 parts per upload. So if you upload a 50GB file there will be 5MB chunks that can transfered, the file of 60GB will fall apart because that will be 10,001.
* The Dynamic Formula -> Always calculate the part size dynamically: Total file size / 10,000 .
* The Sweet Spot -> Set a minimum floor of 16MB to 32MB. This allows files upto 320GB while keeping chunks small enough so that the retrying is fast and cheap.

The Full Architecture (Pre-signed URLs + Multipart) :-
* Init (Intent) -> The browser asks the backend for the file upload. The backend generates a chunks size, calls Create Multipart, saves a Pending row in DB, and generates a Pre-signed Put URLs for transferring the chunks of the file.
* Parallel Upload -> The browser uploads parts directly to the buckets.
* Complete -> The browser tells the backend “it’s done”. The backend calls the Complete Multipart on S3 and runs a Head to verify if all the chunks are there, and marks the DB and Completed.
* Resuming -> If the user refreshes the page, the backend asks S3 which parts are already there (ListParts) and only issue the pre-signed URLs for the remaining ones.

Implementation Notes :- 
* Batch Signing -> Don’t pre-sign 10,000 URLs upfront(not at the same time). Sign them in batches (e.g., 100 at a time) to keep the JSON response small.
* Concurrency -> Keep 4-6 parallel upload at a time to avoid choking the network bandwidth.
* Memory Safe -> The browser’s File.slice() is lazy; it doesn’t load a 5GB file into the RAM, it loads only active chunks.
* Cors Fix -> You must expose the Etag Headers in your bucket’s CORS config, otherwise the JS can’t read it to complete the upload.

Hidden Traps: The ETag Trap & Abandoned Uploads :-
* The Etag Trap -> For standard single file upload, The Etag is a normal file checksum. However for the multipart uploads, it become a special “hash of all the parts Hashes” and ends with a hyphen and chunks counts like (-46).
* Abandoned Uploads (Invisible Billing) -> If a user uploads a 10GB file of 50GB and closes/leaves the tab then that 10GB sits in the staging area(a place which is invisible and the chunks wait there before transfering). It won’t show in the bucket but you pay for it every month.
* The Fix -> Always set a lifecycle rule on your bucket: “Abort incomplete multipart chunks from the staging area after 7 Days”.

Downloading Files: Never Proxy Through Your Server :-
Never pass file download from the backend. It wastes expensive CPU and Bandwidth.
* Pattern 1 (Public Assets) -> Put a CDN in front of a private bucket. Use Content-Addressed Keys (file name is the hash of the content) for perfect naming and automatic caching.
* Pattern 2 (Pre-Signed GET) -> For Private Files the server generates the time-limited Pre-Signed GET URL. To prevent security concerns.
* The Cache Killer -> A Pre-Signed URL’s timestamp makes every request looks brand new making cache hit 0%.
    * FIX -> Sign the URLs to expire on the top of the hour like at (5:00 AM), or use native CDN “signed Cookie”.

Range Requests & Segmented Streaming :-
* Range Requests (206 Partial Content) -> Browser can request specific byte ranges like (bytes 1M or 2M). If invalid, it returns (416 Ranges Not Satisfiable). This unlocks :-
    * Seeking -> Dragging a video timeline that requests just the byte ranges.
    * Resumable Downloads -> Re-resuming bytes exactly from where the connection dropped.
    * Parallel Downloads -> Opening 8 Connection for the different byte ranges of a single file.
* Segmented Streaming (HLS/DASH) -> Netflix/Youtube transcode videos in multiple resolutions like (1080p or 480p) and chop then in 3-seconds segments. The player uses Adaptive Bitrate to fetch the right quality segment based on the network speed. To object storage, they are just thousands of normal, small files.

Understanding Cloud Costs :- 
Your cloud bills are driven by three factors -> 
* Storage -> Per GB stored per month.
* Operations -> API calls (PUT / write operations are usually 10x more expensive then GET requests).
* Egress -> When a user download a file from your bucket it is the most expensive bandwidth/data transfer operation in object storage. Some providers like Cloudflare R2 doesn’t charge you for this operations.

