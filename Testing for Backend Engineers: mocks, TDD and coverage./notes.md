A Backend with Zero Tests & What a Test Actually Is :-
A Test is a second program that runs your main program and gives specific outputs, and makes claims about the output the developer expected and what output the code actually gave.
* The Danger of Bad Test -> It is better to have no test than writing incomplete or improper test because it gives us false sense of confidence.
* The Main Goal (Regression) -> The primary reason to write a test is to catch Regression. A regression is a bug that you previously fixed that came back when you added new features.
* Continuous Integration (CI) -> Test are usually run automatically in a CI system (like Github Action), if you try to merge the code, the CI runs the whole test suite. If the test fails, you know exactly that the new code is breaking the old features.

The First Test & How Testing Frameworks Work :-
You don’t have to write testing from scratch. The programming languages provides tools to automate this and some programmers also open-source the libraries for this.
* The Test Runner -> This is a program provided by your language that you are using (Like go test). It automatically scans your entire codebase, find the filename containing “test” or function containing “test”, compiles them into a temporary executable binary, runs it and gives clean pass/fail reports.
* Assertion -> This is the most crucial part of the test. An Assertion is the actual lines of code that complains or fails if certain conditions are not met. For example, if you expect your (output == 20) output to be 20, but it returns 15 so the assertion will throw an error. A good assertion must include a clear failure message that will give an idea that which part of the code failed, so the developers know what exactly failed.

Unit, Integration and End To End Tests :-
The three primary tiers of automated testing are :-
* Unit Test -> Solitary (The isolated test. Only one single function is tested, and all other external dependencies are mocked or faked).
    * Remember every developer has their own understanding about the Unit Test and about what they actually perform.
* Integration Test -> Sociable (The Collaborative Test. Testing function together with real internal components and see how they interact with it).
* End To End Test -> EndToEnd (The Complete Journey. Uses real database, real network and everything runs exactly as it would run in real world.

The Test Pyramid, Test Sizes, and Test Variations :-
* The Traditional Test Pyramid -> Suggests to build a wide base of Unit Test (cheap and fast as it works only as process), then Integration Test (less cheap and fast as compared to the Unit Test as it can use resources attached to the machine like Redis or DB), and at the upper-most comes End-To-End Test (Expensive and slow in terms of CPU, memory and time).
* A Better Mental-Model -> Developers constantly argue about what “Unit Test” exactly is, instead categorize test by what they are allowed to touch :-
    * Small Tests -> Runs entirely inside one process. They cannot touch the network, database, system files or redis.
    * Medium Tests -> Runs in the same machine and they are allowed to touch local DB or Redis files, but they cannot reach to the external internet.
    * Large Tests -> Can touch anything, including the external internet and have no restrictions.
* Scope Eliminates Flakiness -> Flaky test (test that randomly pass/fail) usually happen due to external services, network issues or server clocks. By enforcing strict “small” test that never leave the process, you push the determinism (guarantee or predictability) to its peak.
* Test Variations (The “WHY”) -> You will hear many other testing terms. They are not new types of test; they are just variation or goals that can be applied to any scope (Unit/Integration/E2E):
    * Regression Testing -> Tests specifically written to ensure old, previously fixed bug don’t happen/occur again.
    * Functional Testing -> Tests the application’s feature as it works according to the user’s requirement.
    * Performance Testing -> Testing is not just speed but, also how much resource the program is consuming under load.
    * Security Testing -> Testing if the program attempts to bypass secure containers, restricted filesystem or mishandle cookies or privacy.  

Test Doubles (Dummy, Stub, Spy, Mock, Fake) & Mocking Rules :-
* Test Double -> When your code interact with the external service (Like Postgres DB or Email service), you cannot use real service in a Test. Instead, you provide a “Stand-in” that looks and acts like the external service. This is called a Test Double (Like instead of actors there body doubles performs the stunt for the security reasons just like that).
* There are 5 types of test double but lazily developers call all of them “mocks” :-
    * Dummy -> Passing a fake value into a function because the function’s signature requires them, but it is never actually used anywhere.
    * Stub -> A double that always gives the exact pre-written response, no matter how many times or what you ask it. (Like giving exact pre-defined information when the User_id == 4).
    * Spy -> It is exactly like the “stub”, but it also records the changes you made to it. (Like your code tries to change the name of the user from X to Y).
    * Mock -> Has strict expectations set beforehead. If your code doesn’t interact with it exactly as configured (e.g., if it is not called exactly one time), it will actively fail the test.
    * Fake -> A properly working implementation that is not suited for production (e.g., using a in-memory RAM instead of a real postgres DB).

State Verification vs. Interaction (Behavior) Verification :-
* State Verification (Best)-> Checking the final result after the code has run.
* Interaction Verification (Avoid) -> Checking the internal steps the code took (e.g., was the public function called exactly once?). Avoid this because this creates “change detector test”, if you slightly improve your code like combining something, it will fail then also as if there is no actual bug in codebase.

Dependency Injection & The Functional Core :-
* The Problem -> If a function creates its own database or network connection then you cannot create a test-doubles from outside for that.  
* The Solution (Dependencies Injection) -> Instead of a function creating its own dependencies, pass them in through arguements/parameters.
* Functional Code / Imperative Shell -> Always structure your code so that all the calculations and bussiness logics remains at the “core”, and all talking to the outside world is kept in the thin shell around it. 

Testing with Real Databases (Testcontainers) :-
* The Problem with Fakes -> Many developers replace the DB with an in-memory map/struct to make the test faster. However the Database bugs (e.g., Bad Transcation or missing index) lives in the DB layer. If you fake the DB, you will lose accuracy and miss these bugs.
* The Solution -> Use a real database but make it fast and user-friendly.
    * Transaction Per Second -> Start a transaction before the test, run everything, and then rollback the changes without commiting them. The DB remains clean for the next test.
    * Template Database (TestContainers) -> A tool called testcontainers spins up a real postgres docker container inside your test code. It uses a “template database” to instantly copy a fresh DB test in milliseconds, allowing the test to run in the parallel safely.

Network, Time, & Hermetic Tests :-
* Network -> When testing HTTP calls, instead of calling a function internally, you can start an actual local HTTP server on a test port. Like in Node.js, msw (Mock Service Worker) is famous for intercepting network layer calls.  
* Time and Flakiness -> Waiting for the background task using sleep(100ms) cause inaccurate test as the busy server of CI/Github Actions might take 110ms.
    * Fix 1 -> Wait for the condition/signal (e.g., GO Channel or Node.js Callback) instead of making abstraction of time duration like this will take 5sec.
    * Fix 2 -> Pass a mock “clock” instace to control time manually like fast forwarding something to decrease the waiting period.
* Hermetic Testing -> A test is “hermatic”(Frozen/isolated) if it brings its own complete environment. It starts everything it wants (DB or server) and stops them once done, without relying on the pre-existing external setups.

Test-Driven Development (TDD: Red, Green, Refactor) :-
* TDD is not just a test; It is a workflow where you write the specification (test) before the application logic.
* The 3 Stages (Colors) :-
    * Red -> Write a test for code that doesn’t exist, it will fail (this forces you to design your API from outside-in).
    * Green -> Create the fastest, ugliest code just to make it pass.
    * Refractor (Crucial) -> Clean up the ugly code, if the test still pass you are safe.
* When to use TDD -> Great when you know how the input/output will look like.
* When NOT to use TDD -> Bad when you are experimenting, exploring or don’t know yet how the final architecture will look like.

Flaky Tests & Their Causes :-
* A flaky test is one that passes or fails on the same exact code. it is worse than having no test because it creates the mindset of the developer to ignore faliures and click “retry” (like if a fire alarm will ring everyday you will take it as “huhh just leave it rings everyday” and when it will run when there is legit fire then you will ignore it).
* Top Three causes :-
    1. Async wait (bad sleep timers).
    2. Concurrency (two things race).
    3. Test Order Dependency (test B only passes when the test A have executed and left some state behind).
* The Fix for Test Order Dependency -> If a test breaks when run in parallel, either delete it or quarantine it (move it to a seprate suite with the developer’s name and reason attached). Never use auto-retries.

Code Coverage & Mutation Testing :-
* Code Coverage (Line vs. Branch) -> Coverage tools rewrite the code to insert counters and see if a line executed. Line Coverage checks if the line ran; Branch Coverage checks if both True/False path of an “if” statement ran.
* The 80% Myth -> Having 100% coverage doesn’t mean your test are good (A test can execute a function without checking the assertion). Treat coverage as a guide to see what is not covered, not a mandatory metric.
* Mutation Testing (Stryker) -> This is testing your test. Tools like Stryker adds “mutants” (damage your code) like “+” to “-“ and more. If your test suite fails, it “kills” the mutant (GOOD). If the mutant still survives (BAD), it means that your test are not actually catching logic changes).

Code Complexity (Cyclomatic vs. Cognitive) & Tooling :-
* Cyclomatic Complexity -> Count the number of independent path through your code (e.g., every if, else or switch case adds a point). Even if the code is human readable and easy but the rules says (Rule of Thumb) to keep the number of switches under 10.
* Cognitive Complexity -> Measure how hard the code is for humans to read. It punishes deep nesting heavily, while ignoring the flat switch statements.
* Linters vs. Formatters vs. Static Analysis :- 
    * Linter (e.g., ESLint) -> Checks for bad anti-patterns, it doesn’t check syntax or bussiness logic.
    * Formatter (e.g., Prettier) -> Only standardlizes the visual layout of your code.
    * Static Analysis -> Advance linters that trace the flow of the single value through the whole program to find complex bugs without running code.
   

