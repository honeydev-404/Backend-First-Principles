1. Traditional Text Search (SQL LIKE / ILIKE)
Text search is the process of finding records with a specific keyword in a column (e.g., finding "Bobby" in a Name column).
* LIKE Operator: A PostgreSQL operator used to find specific patterns. It uses wildcards:
    * % (Percentage): Matches zero or more characters. Example: LIKE "John%" returns "Johnny" or "Johnson".
    * _ (Underscore): Matches exactly one character. Example: LIKE "H_T" returns "Hat" or "Hot".
* ILIKE Operator: The case-insensitive version of LIKE.
    * Example: ILIKE "Tech%" matches "technology", "TECHNOLOGY", or "Technical".
    * You can search anywhere in a sentence by wrapping the word: ILIKE "%Laptop%".
* Limitations of LIKE / ILIKE:
    * Slow on large datasets: It performs a character-by-character scan of every single row (Sequential Scan).
    * No relevance ranking: It just returns matches in a random order; it doesn't know which result is the "best."
    * Basic matching only: A single typo will result in 0 matches.
* Takeaway: Only use ILIKE for small datasets and basic matching. It is not ideal for large, complex applications.

2. Full-Text Search and the Inverted Index
Full-Text Search is a technique for finding relevant information across text documents rather than just pattern-matching in database columns.
* Why we need it: If a bookstore has millions of books, asking the librarian to read every page of every book to find "Machine Learning" is painfully slow.
* The Inverted Index: Instead of mapping a document to its words, it maps words to documents.
    * It is a lookup table.
    * Example:
        * Machine -> Doc 1, Doc 2
        * Learning -> Doc 1
        * Rootkits -> Doc 3, Doc 4
* Instead of scanning millions of rows, the engine instantly checks the Inverted Index to see exactly which documents contain the word.

3. Inverted Index Construction (The Analysis Pipeline)
Before text is saved into the Inverted Index, it is passed through an analysis pipeline to clean it up:
1. Tokenization: Breaks sentences into individual words. ("Intro to Machine" -> ["Intro", "to", "Machine"])
2. Normalization: Converts tokens to lowercase to ensure case-insensitive matching. ("Machine" -> "machine")
3. Stop-Word Removal: Drops common noise words that don't add value ("to", "the", "is").
4. Stemming: Reduces words to their root form. ("running", "ran" -> "run"). This allows a search for "run" to match documents containing "running".
5. Postings List Creation: The engine saves the Document IDs and the exact position of the word to allow for exact phrase matching.

4. Introduction to Elasticsearch
Elasticsearch is a distributed, blazing-fast search and analytics engine built on top of Apache Lucene. It uses inverted-index technology to power full-text search.
* JSON Documents: Data is stored as JSON objects (like records or rows in a database).
* Near Real-Time (NRT): When a document is added, it becomes searchable within ~1 second.
* Horizontal Scaling: Large data is split into pieces called Shards, which can be distributed across multiple servers to handle massive traffic.

5. Field Mapping (text vs. keyword)
When sending JSON data to Elasticsearch, you must map the fields to tell the engine how to read them:
* text Type: The engine analyzes, tokenizes, and stems these fields.
    * Best for: Descriptions, reviews, book titles, and article bodies.
* keyword Type: The engine stores the string exactly as-is. It does not break it down.
    * Best for: Exact matches, IDs, status codes (e.g., "status": "positive"), and tags.

6. Relevance Scoring (The BM25 Algorithm)
Elasticsearch doesn't just return matches; it ranks them using the BM25 algorithm. The score (_score) is calculated using four main factors:
1. Term Frequency (TF): How often the word appears in the document. (More appearances = higher score).
2. Inverse Document Frequency (IDF): How rare the word is across the entire database. (Rare words like "microcontroller" boost the score heavily; common words like "the" do not).
3. Document Length: Shorter documents matching the keyword score higher than massive books matching the keyword, because the short document is more focused.
4. Field Boosting: You can tell the engine to prioritize certain fields. (Example: A match in the Title is worth 3x more points than a match in the Description).

7. Typo Tolerance (Fuzzy Search)
Standard databases break if the user makes a typo. Elasticsearch handles mistakes gracefully.
* Levenshtein Edit Distance: It calculates how many character changes (insertions, deletions, or substitutions) are needed to fix a word.
* Example: If a user searches for "lapto", Elasticsearch realizes it is only 1 edit away from "laptop" and returns the correct results.
* Use Case: This powers the "Type-Ahead" and "Autocomplete" features on websites like Amazon and Google.

8. The ELK Stack (Logs & Observability)
Elasticsearch is widely used for monitoring server logs, not just e-commerce search bars.
* E - Elasticsearch: Stores, indexes, and searches the log data.
* L - Logstash: Collects, cleans, and processes the log files from your servers.
* K - Kibana: A beautiful visual dashboard to generate charts, graphs, and system alerts.
* Bonus: If your company already uses ELK to monitor servers, you can easily use that same Elasticsearch setup for your application's search bar.

9. Performance Benchmark (ILIKE vs. Elasticsearch)
In a test comparing a Postgres database to Elasticsearch using 50,000 rows of data:
* Specific Query (e.g., "laptop"):
    * Elasticsearch: ~500 milliseconds.
    * Postgres ILIKE: ~3 to 4 seconds.
* Broad Query (e.g., "only" - resulting in 8,000+ matches):
    * Elasticsearch: ~500 milliseconds.
    * Postgres ILIKE: ~7.5 seconds.
* Why? Postgres degrades rapidly on broad terms because it must scan the entire disk. Elasticsearch stays flat and fast because it just reads the in-memory inverted index.

