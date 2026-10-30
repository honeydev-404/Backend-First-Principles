The Big Picture (What they are) :-
* These are not specific tools; these tells us what is happening in the backend application and servers that are runing miles away.
    * Loggining (The Diary) -> This is the best practise of recording every important events that happens in your application. It acts as a journal, if somethings breaks it helps in figuring out who did what, and when.
    * Monitoring (The Dashboard) -> It helps us to check the overall health of the server in real time like 10-15 sec intervals. It shows CPU usage, RAM usage and disk space.
    * Observability (The Dectective) :- Monitoring tells us that something is broken up but Observablity tells you exactly why it is broken. A system is “observable” is you can understand its internal state with external output.

The Three Pillars of Observablity :-
* Pillar 1: (Logs) -> The detailed text output of a specific event.
* Pillar 2: (Metrics) -> It doesn’t care about the individual it cares about the big picture of the average success rate or average response time.
* Pillar 3: (Traces) -> Think of the trace like a GPS attached to the single user’s request, if a user click submit button then the request travels to routing layer then validation layer then service layer then finally to the database layer, recording exactly how many milliseconds it spent on the each stop.

The Golden Debugging Workflow :-
* Backend engineer uses these threee pillars when the server crashes.
    * The Alert (Metrics) -> You get a slack notification at 2 AM that 80% percent of the payment processes are suddenly failing.
    * The Search (Logs) -> You open a dashboard like Grafana and filter logs to only show errors from the last 10 minutes to see what it exactly says.
    * The Fix (Traces) -> You click on one of those failed logs to open its Trace, The trace shows the exact path that it passed the validation layer but failed at the line 69 where it is supposed to talk to the database.

Logging Best Practise :-
* Logs should be categorized in levels and formatted correctly depending on where the app is running.
* Log Levels :-
    * Debug -> Extremely detailed logs meant for only local computer to help build the app and turned off in production.
    * Info -> General successful bussiness events. (e.g., “New user account created”).
    * Warn -> They are not quite errors but should know about them. (e.g., “User typed the wrong password”).
    * Error -> Actual system faliures. (e.g., “Database connection lost”).
    * Fatal -> A catastrophic error that forces and entire application to shut down or restart.
* Structured vs. Unstructured Logs :-
    * Local Developement -> Use unstructured logs (colour-coded, plain text printed in console). They are easy for human to read while coding.
    * Production -> Use structured logs (JSON format). JSON is terrible for human to read, but it is good for automated logging tools.

Instrumentations & OpenTelementary :-
* Instrumentation -> It is the act of writing code inside your software to meansure your functions or to measure its performance, track errors and log active data.
* OpenTelemetary -> This acts as a bridge between the backend applicatin and tools like grafana, if you want to shift to Datadog you don’t have to write custom code.

