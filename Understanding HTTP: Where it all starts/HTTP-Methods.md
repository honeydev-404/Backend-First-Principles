HTTP Methods :-
There are various HTTP methods that allows us to retrive, create, delete, replace resources:-
  1. GET -> Used to Retrive data or resources and it is Idempotent.
  2. POST -> Used to Create resources and it is Non-Idempotent.
  3. PUT -> Used to Replace something fully not like Patch and it is Idempotent.
  4. PATCH -> Used for Paritally updating some parts of a resource.
  5. DELETE -> Used for Deleting resources and it is Idempotent.	
          
- NOTE -> Patch does not guarantee Idempotency.
