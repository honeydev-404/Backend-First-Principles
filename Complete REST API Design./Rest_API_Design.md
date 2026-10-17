REST API Fundamentals :-
* API also known as Application Programming Interface decides how a system or software can connect and communicate to one another.
* Rest API(Representational State Transfer API) is an architectural style that is made of set of rules, uses HTTP for communication and defines how an API should be designed so that it works efficiently and provides intuativity, easy to use and predictablity when debugging.
* It should be designed as per the standards of the web service so that is provides ease of developement and increase in client adoption.
* Six Architectural constrainst of RESTful API are :-
    * Uniform Interface -> This is the main function that differs if it is Restful API or Non-Restful API. It suggest that there must me uniform way of interacting with a given server irrespective of devices or type of application.
        1. Individual resources must be specified in the resource, in-short it means the request should contain enough data to get info about the user’s want or which resource he wants.
        2. The client have representation of resource and the request should contain as much data as it allows the user to modify, delete or create data on the server and should be provided the permission to do that kind of operation over the resources of the server.
        3. Each request must contain enough data so that the server can understand the want’s of the user and it must be described so that the server can process the request.
        4. It should contains links for the responses so that the client can browse other resources easily.
    * Stateless -> In REST, the client must provide every information for the server to be able to process the request, like URL, Headers, Metadata etc. The statelessness removes the overhead from the server to maintain, store or modify the session state, but this also have some drawbacks like it requires more bandwith if the data transfer is huge.
    * Cacheable -> In REST, the server must include wheather the response is cacheable or not and if it is cacheable then for how much duration. Some of the very frequent data requested by the user gets saved in the user’s web browser, so next time when the user will request for the same resource then the server can return it from the client’s cache if server passes security concern and gets recognized by the clients brower. This leads reduction in the heavy computation on the server side and improves availability and performance.
    * Client-Server -> Rest application should have client-server architecture. A Client is somone who is demanding the resource from the server ,client should be not concerned about its bussiness logic or its session state. A Server is somone who is holding data, it should not be concerned about the clients cookies or frontend UI.
    * Layered System -> An application architecture needs to be composed of multiple layers so that the work distribution becomes efficient, and there are many layers in the backend application that has there own responsibilty and concerns to handle so any layer must not be concerned about the work of other layer it should focus on its own job. It may helps us enable load-balancing and providing shared caches. 
    * Code-On-Demand -> It is optional but learn conceptually, it is the part of REST and happens when the server provides some executable code to the client to extend its functionlity.

REST - Representation / State / Transfer :-
* Representation -> How resource is represented , and in which format. Example :- {JSON, XML, HTML}
* State -> The resource is in which condition or state, or what is current state of the resource.
* Transfer -> Exchange of resource representation between client and server.

REST API URL & Resource Design :-
* URL Structure :- http://api.example.com/v1/users/143?sort=name#profile
    * http -> It is just a protocol for communication and its more secure version is https.
    * api ->  It is the subdomain (An additional prefix that identifies a section on the main domain.)
    * example.com -> it is the domain (Specific name that identifies the website on the internet.)
    * v1 -> It is called API versioning, it is used when you have more than one version to coexist and you want the user to migrate to the second version that can be v2 and when the user will migrate to v2 then the v1 version will be removed. It is kept temprory to give time to users to migrate to ther second domain.
    * users -> It is the path of the exact resource we are trying to extract.
    * 143 -> It is the user id (users/:id).
    * ? -> It is the seperator for the query parameter.
    * sort=name -> It is the query parameter, it allows us to set presentation of the data as per our preferences like we can sort it by asc/desc or find by name etc.
    * #profile -> The # here is the fragment identifier that direct the browser to the specific part or element on the web page. 
* Scheme -> Remember http or https is just protocols for communication and doesn’t play any role for finding the data, it is done by the rest part of URL.
* Slugs -> If we have to write names of book like Harry Potter then you have to write like harry-potter(kebab-case). It is written like that because our motive is to use least capital letters on the URL. 
* REST API URL should be designed as noun not as verb (You will understand this by example below), and use plural form like /Users.
    * Bad -> /getusers , /deletebook/12 , /removeuser/45 .
    * Good -> /users/12 , /books , /users/:id .
* A proper , clean API URL carries a specific structure and easy to understand heirarchy.
* It should handle relationship in nesting routes and also make it easy to understand. Example -> /api/users/12/posts.
* it should use query parameter of pagination, filtering and sorting. For example -> 
    * Filtering -> /api/users?name=Bobby.
    * Sorting -> /api/users?sort=asc.
    * Pagination -> /api/products?page=2&limit=40.
* Consistency in capitalization improves readablity and some of there uses are in :-
    * Path Segments -> kebab-case (Lower case + hyphens).
    * Query segment -> firstName or first_name.
    * File Extension -> Do not include them in URL. Use accept headers for them.

HTTP Methods + Idempotency :-
Method	Typical Use	Idempotency
GET	Retrive/fetch.	Yes
POST	Create.	Generally No
PUT	Replace resource.	Yes
PATCH	Partitial update.	Yes
DELETE	Delete	Yes

* Idempotency means making multiple request and getting the same server-side effect as performing it once.
* Use GET to retrive data like seeing all the post of the person by clickin there profile and use POST to make a new data/resource like posting a picture on any social media platform.
* Most of the beginner make mistakes when deciding what to use when we have to replace whole data or perform partial update.
    * PUT -> It is used to replace whole data (remember whole data).
    * PATCH -> It is used when we have to perform update on some specific parts.
* DELETE -> It is used to remove a resouce from the server.

CRUD API Design :-
* First of all let’s unlock its fullform -> 
    1. C -> Create -> POST.
    2. R -> Read -> GET.
    3. U -> Update -> PATCH/PUT.
    4. D -> Delete -> DELETE.
* Let’s take an example of organizations to understand it better ->
    1. GET -> list all organizations.
    2. POST -> Create now organizations. 
    3. GET (/organizations/:id) -> list a specific organizations with given id.
    4. PATCH (/organizations/:id) -> Update an organizations.
    5. DELETE (/organizations/:id) -> Delete an organizations.
Same path + different HTTP methods ->
*  If the path is same then the HTTP method will determine which action should be performed when being requested. 
Request body for Create/Update -> 
* Remember, when creating/updating a resource the required data is sent in the request body like JSON etc.
Response status codes ->
* Some important code to remember when designing CRUD mechanism :-
Codes	Meaning
200	Successful request. 
201	Resource created.
204 	Successful request, no response body.
400	Bad request maybe wrong syntax or other
401	Authentication missing or invalid.
403	Authenticated but not enough permissions.
404	Resource not found.
500	Server hit a generic error and doesn’t know how to handle it
* Examples :-
POST /organizations	201
GET /organizations	200
GET /organizations/123	200
PATCH /organizations/123	200,204
Delete /organizations/123	204
GET /organizations/123	404

Missing Resource vs Empty Resource :-
* Missing Resource -> The requested url points to something that doesn’t exist or connot be found. It occurs when there is undefined variable or file that was deleted, typical status code is 404 Not Found. Like {GET /organizations -> No organizations -> 200 Ok -> [] }
* Empty Resource -> The requested URL, exists but currently contains no data. An empty string “” or an empty array [], typical status code will be 200 Ok or 204 No Content. Like {GET /organizations/123 -> 123 doesn’t exists -> 404 Not Found }.

List API’s :-
Pagination ->
* If you request some sort of data and it is in very large files like 1000 pages so you will get response delay because of heavy serialisation and deserialisation work, so the server returns the data in chunks.
    * limit -> how many per page.
    * page -> from which chunk.
* ?limit=2&page=2 ---> so this will return items from the 2nd page that is page parameter and from the newest to oldest (as in most of the case it is default preset.) and in the 2-2 pair from the 2nd page that is limit parameter.
* A paginated response contains these fields :-
    1. page -> “These items are from which page”.
    2. limit -> “In how much parts/chunks the data will be returned”.
    3. total -> “The absolute count of items on the DB {So the UI can show ‘from 10 of 50’ }”.
    4. totalpages -> The maximum number of pages. The frontend uses this to know when to stop to prevent infinite scrolling, when page === totalpages, stop.
* The backend have some sane-default values such as page to 1 and limit to 10 or 20 something.

Sorting :-
* There are main two params in sorting :-
    * sortBy -> “From which field” like name or age etc.
    * sortOrder -> “In which order” like (ascending/descending).
* There are also some default values in this also like sortBy=createdAt and sortOrder=desc (From newest to oldest.)

Filtering :- 
* It is used to filter information or the fields by some parameters or keywords.
* Example -> ?status=active , ?status=archieved&name=co.

Custom Actions in Rest API’s :-
* The actions which are not included in the CRUD operation and only include HTTP methods, they can not fulfill all the operations like approving an invoice or cancelling an order. These operation can be handled by designing some custom actions.
* A custom action is used when an operation cannot be respresented by the standard CRUD operations.
* In most of the case when we need to perform a custom operation we use POST method because it is inherently non-idempotent and designed for executing arbitary operations and it server as the universal escape from the hatch for non-CRUD bussiness logic.
* There are many ways to perform custom actions :-
    * Using POST with custom actions -> Example [POST /users/{id}/activate] , this will activate the user account. This is easy to implement and understand but it comes with a cons that the overuse of POST can lead to the blurring of the create and non-create opertions.
    * Using Custom HTTP Methods -> Example [ MOVE /files/{id} ] , this will move files from one place to another. This provides clear and explicit way and keeps us away of using POST for custom operations, but the cons is custom methods may not be supported by all the clients and can lead to inconsistency with different API’s.

Consistent Naming & JSON Field Naming :-
* Casing Standards -> Pick one standard and use it across the entire system, either use camelCase or snake_case.
* Semantic Clarity -> Use booleans with clear state prifixes like is_active, has_Access and is_deleted. For timestamps, always use ISO 8601 format (YYYY-MM-DDTHH:mm:ssZ) and standard naming suffixes like created_at or updated_at.
* Standardize Enums -> Use uppercase or use explicit standard values like (Status : “PENDING” or “pending” or 1).

Consistent Routes :-
* Noun Over Verbs -> Use plural nouns over verbs because there is not a single user which is using the services.
* Avoid Anti-Patterns -> Never create routes like [GET /api/GetUserById —> Instead use Get api/user/{id}].
* Nesting and Hierachy -> Don’t make the routes too big instead use sub-resources.
    * Good -> GET /users/{UserId}/orders .
    * Bad -> GET /users/{UserId}/orders/{OrderId} - - - and so on.

Sensible Defaults :-
* Paginations -> Never let the endpoints dump all the tables at once, hardcap the default value as maxlimit=100 and as page=1 and limit=20.
* Sorting and Filtering -> Set the dafault sorting values as sortBy=desc to see the recent entries.
* Optional Body Payloads -> If the incoming field is not mandatory you can set some dafault values to that like {status = “active”} rather than keeping it as null values.

API Design-First :-
* First of all discuss all the ideas and implementation on the backend and frontend side rather than starting to write code first and wasting time debugging and connecting things that doesn’t mattter or decided.
* Steps :-
    1. Define the API specs.
    2. Review the specs and the demand of the consumers.
    3. Spin the mockup servers from the specs.
    4. Frontend builds against the mock while backend implements the database and business logic in parallel.

Swagger/OpenAPI :-
* OpenAPI -> It is the standard format of describing the API. It is publically available application interface that lets outside developers access a software tool or webservice.
* Swagger -> It is used to present documentation in readable/testable interface.It documents the endpoints or operations etc so that both the human and machines can understand the API without looking at the source code.
* Postman/Kong Insomania -> These are the two GUI based applications used by the  which are used to design, test and debug API’s.





