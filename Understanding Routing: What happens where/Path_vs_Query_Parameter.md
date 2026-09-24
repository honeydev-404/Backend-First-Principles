Path Parameter vs Query Parameter ->
This distinction is important because it is very confusing for beginners.

Path Parameter :-
* Path parameter are used to identify specific resources.
* Like -> GET /users/123.
* It means that give me this specific user.
* Use path parameter when you have to identify a specific resource. 
* Simple rule use path parameter when “What exact thing”.

Query Parameter :-
* Query parameters provide additional information that can control how the server searches, filters, sorts, or paginates the returned data.
* Like -> GET /users?page=2&limit=20
* These parameters tells us that Page = 2 and limit = 20.
* Query parameters are very useful because they don’t just identify the user but they also help use modify how result will be represented/returned.
* Use query parameter for :-
    1. Pagination :- In how many pages you want to see the result. -> /api/books?page=2
    2. Sorting :- In which order do you want to see the result, it can be ascending or descending. -> /api/books?sort=price
    3. Filtering :- You can filter information about the product with the help of keywords. -> /api/books?genre=fiction
    4. Searching :- You can extract some information about something. -> /api/books?search=harry

