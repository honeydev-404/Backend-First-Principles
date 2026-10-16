Handler/Controller Layer :-
* Handler, sometimes called Controller layer is the first layer of the backend architecture that handles incoming and outgoing request and it is the first layer that interacts with HTTP.It mainly works with two objects :-
    * Request Object -> It contains headers,methods, body, metadata etc.
    * Response Object -> Used to write headers, set status codes, or send back data.
* It have some task that it has to perform and it also gives us idea about the flow ->
    * Extracting data -> GET -  Query Parameter , POST | PUT | DELETE - Request body.
    * Deseralize data -> Rust: JSON - Struct , Python: JSON - Dictionary.
    * Validate and Transform -> Transform data, sanitize inputs and validate data etc. Don’t return unnessary 400 error messages instead provide more user friendly messages.
    * Delegate to Service Layer -> If the validation is successful then proceed further and handover the logical tasks to Service Layer.
* Note :- Controller/Handler Layer -> Handles HTTP Concerns, and validates and transforms data to continue further.

Service Layer :-
* The service layer is the second layer and in this layer all the bussiness logic takes place all the orchestration and data procession takes place in this layer.
* Golden Rule -> The service layer should know nothing about the HTTP and its method,status code and request/response. Just data in from the handler and data processing and data out to the handler.
* It also have some responsibilities :-
    * It should process validated data from the handler.
    * It should perform data processing with many orchestration and other services.
    * it should handle the faliures
    * It should then send the processed data to the handler.
    * Each small function should have dedicated responsibilites like CreateUser() and  SortUsersByDate(). More complicated work can be handle by the orchestration.
 
Repsitory Layer :-
* Repository layer handles all the database related jobs such as filtering,deleting or creating new entries, it should have focused methods for the specific database operations as FindAllUsers() or FindByID(id).
* It should not care or contain knowledge about service layer at all.

Controller vs Service vs Repository Layer :-

Handler Layer	It handles HTTP Concerns and extracts data from the request.
Service Layer	It implements bussiness logic and takes the data from the Handler layer.
Repository Layer	It handles all the database operations related jobs like filtering,deleting or creating.

Middleware :-
* Middleware is a function or the series of function that handle request and response in Backend application. It is not all about HTTP request it modifies the request before it reaches to the main handler layer and alter the responses before it is sent back to the client.
* One critical aspect of the middleware is that order matters.
* Next() -> This function tells that the middleware has done its job and now it can proceed further to next middleware/handler, there can be some consequences if the functionalities fails so there are other ways to handle that.
Why Middleware :-
* It reduces many overheads such as ->
    * Overhead of writing dedicted code for the certain layer so that it can authenticate etc.
    * To make it useable so we don’t have to write a lot for certain functionalities. 
    * We can also terminate the request before it reaches to the main handler.
* Middleware is the in-between layer that let’s you process, inspect or reject the HTTP requests at various stage. And provides the ability to implement certian functionalities between layers.
* So conceptual order and the functionalities of the middleware is :-
    * Authentication/Authorization -> Verfies that the user is allowed to access the resource.
    * Logging -> Check the request informations like request time, IP, monitoring or debugging etc.
    * Error Handling -> Provides consistent messages to client when error occurs in request. 
    * Compression -> Compress response data so that it can be transfered more efficiently and fast.
    * Rate Limiting ->Limits how many requests a client can make within a specific time period.
    * Caching -> Storing frequently fetched data into in-memory so that it can be accessed more efficiently and will take less time.
    * Request Validation -> Validates the request if it is meeting specific requirements, like valid format or required field.

Request Context :-
* Request context is a request-scoped storage that contain key-value pair data each and every HTTP request, it stores the temprory state of data about the request so that many components can share it. And it is request scoped temprory state.
* For Example :-
    * Authentication middleware extracts UserID from JWT.
    * It stores UserID in context.
    * Later handler can access if for DB queries.
* Suppose user have provided random UserID so the auth mv must not trust is blindly it should do some identification first.

Short Summary :-
* Handler     → HTTP concerns
* Service     → Business logic
* Repository  → Database operations
* Middleware  → Reusable request/response processing
* Context     → Request-scoped shared data

Flow :-
* Now the flow looks like this ->
    * request > request context > layers of middleware > handler > service layer > repository layer.

