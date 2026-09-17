Multipart vs Streaming :-
Mulipart ->
* Multipart allows a request/response to contain multiple separate parts of data in a single HTTP message.
* Each part can contain different data, such as a file and additional form fields.
* It is commonly used when uploading files along with other data.

Streaming ->
* Streaming means sending or receiving data piece by piece instead of loading the entire data into memory at once.
* It is useful for large files, videos, and other large data.
* It reduces memory usage because the whole file does not need to be loaded into memory at once.

