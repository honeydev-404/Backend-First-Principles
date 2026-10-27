12. Full text search using Elasticsearch for blazingly fast search.

Traditional Text Search (SQL LIKE/ILIKE) :-
* Text search is the process of finding records with a specific keyword in a column. For example -> you can find “Bobby” from the Users table with Name column.
* LIKE is a PostgreSQL operator that is used to find for a specific pattern in the column.
* The LIKE operator supports some wildcard characters :
    * (%) -> Matches zero or more characters. Example -> If { LIKE “John%” } then it can return “Johnny” or “Johnson”.
    * (_) -> Matches exactly one character. Example -> if { LIKE “H_T” } then it can return “Hat” or “Hot”.
* ILIKE is also an SQL operator used to find matching Case-Insensitive words from the column.
    * Example -> if { ILIKE “Tech%” } then it can return words in both capital and small letter > “technology” , ’TECHNOLOGY” , “TECH SUPPORT” , “Technical”.
* We can search for text anywhere means if it comes between the sentence by using % on both starting and end of the word. Example -> { ILIKE “%Laptop%” }. It will find in between and also it will find case-insensitive also.
* Limitation of ILIKE and LIKE :-
    * It can be slow if we are finding from large dataset.
    * They don’t rank result by relevance.
    * They provide basic matching.
* We should use ILIKE and LIKE for small dataset and for basic matching not keeping it ideal for large ones.
* The main difference in LIKE “%term%” and full-text search is that the full-text search analyzes the text into individual words and ranks result by relevance and the LIKE is slow and it performs character-by-character string scan.

Full-Text Search and Inverted Index :-
* Full-Text Search -> It is a technique of finding relevant information across the text file rather than just matching the basic pattern in database column.
* Example -> Imagine you are looking for a book in online bookstore. A user search for Machine Learning.
    * Traditional search may find only pattern in the database that contain exact matching.
    * Full-text search can analyze the text, find relevant books and rank them with how much they match with the query.
* We need full-text search because just imagine there are a large amount of books, if you ask the librarian about the book he has to check the index, content or description this is very inefficient as the dataset grows and traditional search only find the matching pattern not relevance.
* Full-Text Search addresses these limitations by using an index designed for text searching.
* Inverted Index is a data structure that stores term and references of a book in a document. Instead of looking to the whole library we look to the inverted index.
* An inverted index can look like this :-
TERM	DOCUMENTS
Machine	Doc 1, Doc 2
Learning 	Doc 1.
Rootkits	Doc 3, Doc 4, Doc 5.
x86_64	Doc 6
Coffee	Doc 7
* Elasticsearch -> It is a distrubuted, fast search and analytics engine built on top of Apache Lucene and it uses inverted-index-based technology to power full-text search.
* Another advantage is Relevance Scoring. It can rank the document by the factors like how often the term apper in the document and wheather it appears as a important field or not. Elasticsearch uses BM25 as a relevance-scoring algorithm.

Typo Tolerance (Fuzzy Search and Type-Ahead) :-
* The problem -> Standard database are rigid. If user misspells a word like “tranding” instead of “trending” the database will return 0 results.
* The solution -> Elasticsearch uses context and full-search capabilities to find the word the user actually meant.
* The use case -> This is the exact technology most of the big tech industries uses, that are type-ahead or auto-complete. Like when you browse the items in amazons it gives suggestion.

The ELK Stack (Log Management) :-
* What it is -> Elasticsearch is not only used for searching products only, it is also used for managing and finding errors and more in big server logs.
* The Stack Components :-
    * E (Elasticsearch) -> An analytic engine used to collect and store the index for fast searching.
    * L (Logstash) -> A data collecting pipeline that collects data from the various resources, transforms them in a structured format and ,sends it back to elasticsearch.
    * K (Kibana) -> A visualization layer that runs the top of elasticsearch letting the user to build charts, graph, dashboards and analyse data.
* Why it matters -> Running and maintaing the elasticsearch requires complex setup and expensive servers. It the company is paying to run ELK stack for internal mointoring then you already have a world class search engine.

The Performance Benchmark (The live demo) :-
* Running a test between a serverless postgress database and elasticsearch by uning datasets of 20,000 products reviws :-
* Specific Search (e.g., “Laptop”):
    * Postgres ILIKE -> ~4-3 seconds.
    * Elasticseatch -> ~400-500 miliseconds.
* Broad Search (e.g., “only” in 8000+ matches):
    * Postgres ILIKE -> ~7-8 seconds.
    * Elasticsearch -> ~500 miliseconds.
* The “Why” -> The postgres get slowser as it have to search many rows on the disk while the elasticsearch remains fast as it remains in the inverted index.

