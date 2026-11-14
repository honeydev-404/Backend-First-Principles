The Knowledge That Lives in Three Places (The Problem) :-
* The 3 places -> When you build an API endpoint, the details about what data it takes and what status codes it returns live in three places.
    * Backend Handler -> A specific function in your code that is being written for a specific API route, and when it is called that function executes the task.
    * Frontend Code -> The Javascript or React code that sends the request to the specific Backend API, to handle tasks.
    * API Documentation -> A manually or static file (Like PDF, Markdown or Notion) that is written to understand about the API, so that the new developer doesn’t face any issue understanding it.
* The Syncing Issue -> Because the knowledge lives in three different places, they can easily go out of sync. If a developer updates or modifies the backend handler code but forgets to update the documentation, the new developer will assume that the documentation is correct and implement wrong analogies that will break the API.

A Signature for the Remote Call :-
* Compile-Time Safety -> Inside a single codebase, if you pass the wrong data type in the function, the compiler throw the error immediately before the app even runs.
* No Protection Across the Network -> When the frontend calls the backend over the internet (HTTP), there is no compiler to check mistakes. Mistakes like wrong data type or field names are only caught when the app runs and crash(runtime).
* The Solution (OpenAPI) -> OpenAPI gives your remote HTTP calls a strict “Signature” or rulebook using a central file (mainly in JSON or YAML), so the mistakes are caught before the app even runs.
* Origin Story -> Created in 2010 by Tony Tam at a startup called Wordnik. His realization was that the developers write code much faster than writing a documentation for it.

One File, and the Programs That Read It :-
* Why JSON/YAML -> A plain text file can only be read by humans, by writing it in a structured format like JSON or YAML, software programs can also read it and automate the tasks.
* 4 Superpowers of One OpenAPI File :-
    * Interactive Documentation -> Creates a visual webpage (Like Swagger UI) where developers can read, understand or test APIs live.
    * Auto-Generate Client Libraries -> Automatically generate the code to call the API, many Compliers and CLI Utilities are there to generate the code and the code that is generated we call it as library (SDK -> Software development Kit).
    * Auto-Generate Server Boilerplate -> Generate the backend skeleton.
    * Automatic Request Validation -> Automatically check if the incoming request data is correct, without you writing if-else conditions in your code to check it.

What OpenAPI Is, and Where It Came From :-
* The 3 Core Pillars :-
    * Language Agnostic -> It doesn’t care in which language your backend is written like node.js, python or golang. It works with everything.
    * For Humans & Computers -> Humans read it to read and test APIs; Computers read it to auto-generate code.
    * No Code Inspection Needed -> Frontend developer never need to read the backend code to understand how an API works.
* 4 Commonly Confused Terms :-
    * OAS (OpenAPI Specification) -> The official set of rules of defining how the file must be written.
    * OAD (OpenAPI Description) -> The actual file that follow these rules (Like openapi.json or openapi.yaml).
    * OAI (OpenAPI Initiative) -> The organization that maintains or update these rules.
    * Swagger -> The old name of the OpenAPI Specification before 2016. Today, OAS is a rulebook, and swagger refers only to the UI tool that is built around it.

The Document: info, servers, paths :-
* Minimum required fields :- The openAPI file needs these 3 basic things to get started.
    * Openapi -> The version of the openAPI spec your are using.
    * Info -> Your project’s title and your internal API’s version.
    * Paths -> Map all of your API routes.
* Servers (Base URLs) -> A list of URLs where your backend is hosted (Like in local environment [http://localhost:8080] and in production URL). Writing the base URL here removes the overhead of writing the full base URL for API routes.
* paths (Endpoints) -> List all your routes starting with a slash (/api/move). Inside each route, you list the HTTP method (GET, PUT or POST).
* Dynamic Path Parameters -> If your URL has a Dynamic value that changes, wrap it in curly braces (Like /api/tasks/{id}).

Operations, Parameters, and the Request Body :-
* Operation -> Every HTTP method inside a path is called an operation.
* Tags -> Used to group related API routes together in the documentation UI (So all the user routes will be in one folder).
* operationId -> A unique name for that operation, written as a function name (Like getUsers or moveTask). This is very important because code generator use these names in the frontend and backend function.
* Parameters -> Used for input that are passed via the URL path (Like /api/users/{id}), query string (Like ?since=10), or headers.
* requestBody -> Used when you send some payload (Media) like in POST or PUT. It specifies the media type like photo or video and the schemas (required fields).

Responses and the Schema Object :-
* Responses -> These are listed with HTTP status codes (Like 200 for Success and 500 for server error etc), each error code tells us what happend and the conclusion in 3-digit numbers.
* The Schema Object -> This is the blueprint of your data. It defines the exact structure and rules for every field.
* Data Types -> It defines if a field is a string, interger, boolean or array.
* Validation Rules -> You can set strict rules here without writing code in your backend. Like a status field should have a active or inactive status.

Code-First vs. Design-First (Contract-First) :-
* Approach 1 (Code-First) -> You write your backend code first. Then a tool or library needs your code to automatically generate the openAPI file. This is best when you are adding openAPI file to already existing project.
* Approach 2 (Design-First or Contract-First) -> You write the openapi.yaml specification file first, before writing a single line of backend or frontend code.
* Why Design-First is better -> Once the openAPI file is ready, the frontend and backend team can work at the exact same time. The frontend team don’t have to wait for the backend to be build.

The Design-First Pipeline (How it actually works in real companies) :-
* Step 1 (Lint) -> Use a tool like Spectral to catch formating and syntax error in the yaml file before generating anything.
* Step 2 (Mock Server) -> Use a tool like Prism to instantly create a fake server that returns dummy examples. Frontend can start building UI against this immediatly.
* Step 3 (Generate Backend) -> Use a tool to generate the server boilerplate (interfaces/routes) from the yaml file.
* Step 4 (Generate Frontend) -> Use a tool to generate the client SDK (ready-to-use function) for the frontend team.
* Step 5 (Write the Business Logic) -> The backend developer fills the empty server boilerplate with real database queries. The frontend developer connects the generated SDK(functions) to the UI.
* Step 6 (Documentation) -> Use a tool like Swagger UI to auto-generate an interactive, clean UI to read or test APIs from the exact same file.

Test the Server Against the File :-
* Contract Testing -> Use a tool (Like Schema Thesis) to send auto-generated fake requests to your real backend.
* Purpose -> It checks if your real server matches the rules of your openapi file.

GitHub, Stripe, and the Standard :-
* Industry Standard -> Big companies share their massive OpenAPI files.
* Use Case -> Other developers use these files to auto-generate code and to connect to stripe(payment platform). And even AI Agents use them to know how to interact with an API.

What OpenAPI is, in one sentence :-
* The Problem -> A code compiler cannot catch the error outside the network boundary.
* The Solution -> OpenAPI is a single source of truth document that describes your api. You use this one file to safely auto-generate client SDKs, server code or tests.
 





