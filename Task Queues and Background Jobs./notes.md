11. Task Queues and Background Jobs :-

What is Background Jobs :-
* A Background Job (also known as Background Task) is a piece of code, logic , or workflow that executes or runs besides the Request-Response lifecycle. Like a typical Request-Response lifecycle :- A client request something -> Server processes it -> The response is returned.
* The task that takes place besides the cycle that is shown above is Background Task.
* Remember -> Background Task = Background Job.
* Key Characteristic -> It doesn’t need to be synchronous (means it can be handled besides the request-response lifecycle).

Synchronous vs. Asynchronous Processing :-
* Synchronous processing :- Processes that are strictly and compulsary to be processed within request-response lifecycle is knows as synchronous processes.
    * Example -> If the user is signing into a website so to check his cresidentials is to be processed at the same time so it is synchronous.
* Asynchronous processing :- Processes which can be handled besides or after request-response lifecycle is known as asynchronous processes.
    * Example :- If the user have signed up so the website can send him the welcome email whenever the load is less do it can be queued for later as it is asynchronous.

Why We Need Background Jobs :-
* There are certain reasons why we need background jobs :-
    1. It improves responsivenes by keeping the asynchronous task queued for later.
    2. It prevents timeouts as the user will not wait for too long only because his all the processes are being processed.
    3. It helps isolate slow task from the request-response cycle, while retrues and idempotency helps in handling task faliur and duplicate task execution
    4. Improves scalability as it provides space for the server to process the synchronous processes first.
    5. Better user experience as the user doesn’t have to wait for all the processing some of the can be queued for later when the load is less.

Queue, Producer, Consumer and Worker :-
* They are the sub-components of the Background task. It is divided into queue, producer, consumer and worker, joining all of these makes background jobs mechanism.
    1. Producer / Publisher -> It creates the task, serializes it and push it into the queue. This is called Enqueuing (ENQ).
    2. Queue / Broker -> This middleware stores tasks until workers are ready to process them and manages task delivery. Durability, ordering and delivery guarantees depend on the broker and its configuration, like RabbitMQ.
    3. Consumers / Worker -> A separate process that pull the task from the queue(Dequeuing), and executes the registered handler function.
    4. Handler -> The actual function that is registered for the task type, it contains the real bussiness logic and the function to perform for certain task.

Enqueueing and Dequeueing :-
* Enqueueing -> Add a task and store it in queue/broker for later processing.
* Dequeueing -> A worker retrives a task from the queue.

Retries and Exponential Backoff :-
* Retry is an automated mechanism in which the systems re-attempt the failed operation rather than giving up immediatly. It is designed to handle some transient (errors).Such as brief network drops, rate limits or system reboots.
* Exponentail backoff is a error-handling strategy in which the client waits the double of the previous time. It give struggling server or database the time to heal instead of hammering it with continous request.
* let’s undrestand it visually :-
First Attempt	1 minute 
Second Attempt	2 minute
Third Attempt	4 minute
Fourth Attempt	8 minute

Acknowledgement (ACK) & Visibility Timeouts :-
* When a consumer picks up a tasks from the queue (Dequeues), it is not deleted immediatly it just gets hidden from other workers, this is visibility timeout. In this period a task is considered to be “in progress”.
* If a worker crashes, task recovery depends on the queue's acknowledgement and failure-recovery mechanism.
* If a task is not acknowledged before the visibility timeout expires, it may become available to other workers again, depending on the queue's configuration.

ACK (Success) :- Consumer sends acknowledgement -> Queue permanentely deletes the task.
NACK (Failure): The worker explicitly informs the queue that it could not process the task. The queue may retry or requeue it, depending on its configuration.

Error Handling :-
* If the email sevice is unavailable, record the error so you can diagnose later.
* A temprory error like external API is down can be resolved by retrying but an application-logic error can not be fixed by retrying.
* Decides for how many times a task can be retried and how much time the user have to wait for the next attempt.
* If a task keep falling and reaches its maximum limit then define what happens next rather than retrying infinitely.

Common Use Cases :-
1. Email Delivery.
2. Push Notification.
3. Cleanup/maintainence.
4. Image/Video Processing.
5. Report Generation.

Types of Bacground Jobs :-
1. One-Off Tasks -> Triggered once by a specific event. Send verification email, welcome emain or passwork reset-link. Most common type.
2. Recurring Tasks -> Schedules at fixed intervals, example -> monthly report, Cron-Jobs or cleanup jobs.
3. Chain Tasks -> Parent-child dependencies , Task B can only after Task A succeeds. Task at the same level can run in parallel, example -> generate thumbnails; transcript may run in parallel while encoding.
4. Batch Tasks -> Handles a large number of records or items and shuffle into groups rather than handling each item individually, example -> generate the reports of 10,000 users.

Idempotency :-
* Idempotent -> An operation is idempotent when running it multiple times produces the same intended final effect as running it once, without unwanted additional side effects.
* Why it is important -> Reduction in performing duplicate work or modifying the db values or charging twice the price.
* Side effects -> It can cause unwanted side effects on the database, send mails twice or charging twice the price.
* Core Rule -> Repeating an idempotent opertion cause no duplicate or unwanted side effects.

Monitoring, Logging and Alerting :-

Monitoring :-
* It is the process of continously tracking the performance and health of the background job system.
* For example, imagine your backend has a queue that process welcome emails. You need to track :-
    1. How many tasks are waiting to be processed.
    2. What is the task status.
    3. How many task successed without retries.
    4. What is the success and faliure ratio.
    5. What is the worker health

Logging :-
* Logging is the process of recording events and information about the task execution.
* Logs provides us detailed overview that what was the point of faliure and what was its cause. Like -> External-Service Faliure.

Metrics and Dashboards :-
* Metrics are the numerical measurement that describes the performance and health of the system.
* A dashboard present three metrics visually, often using chart, tables or graphs.
* Example of tools -> Grafana and Prometheus.

Alerting :-
* Alerting is the process of informing responsible people about the important issues occur.
* For example -> A worker becomes unhealthy and can fail anytime.

Scaling and Design Consideration :-

Horizontal Scalling :-
* It means adding more server or worker nodes for better work distribution.
* The database and external services must be able to handle the additional workload.

Task Ordering :-
* It ensures tasks execute in the required sequence when the queue or workflow system supports and is configured for ordering.
* If a user is creating an account then the welcome mail should be sent after the user is registred.

Rate Limiting :-
* It controls how many request can be in a specific period of time by a server.
* It protects the external sevices by capping the rate limiting.

Smal, Focused Task :-
* They are background tasks designed to perform one clear unit of work.
* For exampe -> There are 10000 images in one large task you can divide them in certain parts.
* They make faliure easier to isolate.

Task Dependence :-
* It describes a relationship where Task B must wait for Task A to complete or provide its output before proceeding.
* As the video is getting encoded when it is encoded then it will generate thumbnails and then the metadata needs the encoded output to process further.
* Chains and workflows help manage dependent tasks.

