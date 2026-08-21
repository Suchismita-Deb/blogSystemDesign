### In the project two different macro services are showing different data for the same user. The user service showing one e-mail address and the order and payment service showing old e-mail address. How will you identify the root cause and fix the data inconsistency ?

The first step is to identify the source of truth for that data. If the user profile belongs to a user service then user service database should be considered as the correct source and correct data.
 
The next thing to see that how other services are getting this data usually one service keeps a local copy using an event.  
So the first thing is to see the update event is published properly or not. Then we have to see whether the consumer service received that processed event or not.

In case the event was missed or failed then we have to reprocess that event. We need to see the Kafka topic, the lag, the failed message, the dead-letter-queue, the lags and the timestamp.  

To avoid these kind of issues from further occurrence we need to add a veteran mechanism, dead letter Q, proper monitoring and alerts. In case of important data we also need to create a reconciliation job which compares data between services and fixes the mismatch. 

The first thing to do - finding the source of truth, trace the event flow, fix the fail event and add monitoring so that the issue can be caught early next time.

### In the project there are two micro services that are trying to update the same customer data at the same time. One service is updating customer profile and another service is updating customer verification status. How will you avoid wrong data updates or data overwrite?  
 
The first thing to do is to use optimistic locking and keep a version column in the table. When any service is updating the customer record it will see the current version if the version is same then update would be successful and version will be increased. In case the next service has already updated the same record then the version will be changed and in that case the second update would be failed and we'll retry with the latest data. 

 
In case of critical data it should be pessimistic locking but generally it's not advisable because it can slow down the system.  
I would also like to use the idempotency key for important operations so the same request does not update the data multiple times.


### There is one Microsoft review that publishes event to Kafka but there are some bugs and it starts sending data to multiple services and it's wrong data. How do you stop this bad data from spreading across the system ?

The first step is to follow the rule that consumer services should not process every data and every event blindly. Every consumer should validate the data before processing.  
The consumer should verify if the schema is valid, the data is present and in correct format, the required fields are present, the event version is correct.  
It should also verify the business logic suppose the amount cannot be negative, the ID cannot be empty, the status should be valid Should be valid and in all other cases it should not process the data and send to the dead-letter queue and send an alert.

In case one producer services continuously sending bad events then we should temporarily stop consuming from that topic or disable the consumer. We should also use some idempotency so even if duplicates are wrong even come again we should not update the data again and again.

### There is one API gateway in the system and all the requests to order service, payment service, inventory service and notification service goes via that API gateway but the request fails somewhere in between. How will you trace exactly where the request failed ?
The first thing to maintain is that to use a correlation ID or a trace ID. When a request enters the system through API gateway we should generate one unique ID and that I'd will be passed to all the microservice through headers. 

Every service will log the same ID with request and response details and in case if any request fails anywhere we should search by the trace ID in the centralized logs. I should be able to see the complete journey of the request where the service failed, which service was called, what error came and how much time each service took.

For better debugging purposes we should use some distributed tracing tools like Zipkin, Jaeger, OpenTelemetry or any APM tool.


### There is a Kafka topic and we can see that the messages are getting stuck in the queue and consumers are not processing them. How will you debug and fix this message backlog issue? 
 
The first step is to see whether the consumer service is up or not in case if it's down then we should restart and see the deployment issue.

The next step will be to see the consumer logs in case if it's throwing any exceptions while processing a message and also we should monitor that if there is any bad message that is failing over and over again in case if one message is blocking the process then we should move it to the dead letter Q.  
 
The next step is to see the queue size and in case the message is coming faster and consumer is not processing then increase the consumer instance and we need to see the downstream applications like DB and external API calls are taking time so in many other cases the database query is slow and so the consumer is not processing the data at the same speed.

In short I should be seeing the consumer health, logs, bad message, consumer lag, downstream slowness and optimize the consumer accordingly.

### In the project we can see that one internal microservices is accidentally exposed to the Internet and starts receiving unknown and malicious traffic. How will you secure this internal service immediately? 
 
First thing internal microservices should not be directly exposed to the Internet. Only API gateway should be public.  
The first thing should be immediately remove the public access from that service. I need to see the ingress, the load balancer, the farewell rules, security group or the Kubernetes service type to see where that API get that public access and also I have to move it back to private network.

The next step is to also add service to service authentication using JWT, O auth, MTLS, internal tokens. The service should also add some rate limiting into it so that malicious traffic doesn't overload the service.  
 
I would also like to see the logs to see what kind of request came, from which IP, were there any sensitive endpoint was accessed. In case if any sensitive data has a chance to be compromised then the primary steps should be to rotate the secrets, API keys and credentials.


###  The business team wants to release a new feature only for 10% of the user 1st and if everything works fine then it will be released to all the users. How will you design these features rollout ?

The solution is to use feature flag. The feature flag allow us to enable or disable a feature without new deployment. The scenery for 10% rollout we will be using user ID based hashing or bucketing.

I will divide users into 100 buckets and enable the feature only for bucket zero to 9 and this will give me around 10% of the user.  
The feature should be visible to all of these users and I will monitor the logs, error, latency and business metrics. If everything goes fine then I'll be increasing it gradually to 25 percent 50% and then 100%. In case it goes wrong then I'll immediately turn off the feature flag.

### There is one API request that is making too many internal micro service calls and because of that the response is becoming very slow. How will reduce the number of service calls and improve the performance? 
Multiple internal calls meaning like around 10 internal calls is not a good thing as it will make the response time high. In case one service is slow or down then the request will be down.

First thing To verify is that if there is any repetitive call of same data or unnecessary calls, some data can be fetched together, data might not need immediately then that case I would remove those unnecessary calls. 
The next step would be using API composition if multiple services are needed to prepare one response then API composition can be used it means that one layer will collect the data from required services and prepare the final response. 
The next step is to use caching for the data that doesn't change frequently. Example product details, user basic profile, configuration or master data.
The next step is to move the non critical data to asynchronous processing period for example e-mail notifications, audit logs, report updates, analytics can happen in background.
The 5th step is that if the service needs another service data very frequently then keep the local copy using event driven data replication.

### The application is migrating from monolithic to microservice, but there are many modules which are tightly connected with each other period how will you safely break these modules into separate microservices? 
 
The main thing is that I will not directly split all the modules at once. The first thing is to understand the business boundaries like user management, order management, payment inventory and modification can be separate business area. 
I will start with one less risky module and move it outside from the monolith first. It is a safe option and we can test 1 service properly before splitting everything. 
 

I will avoid the data database sharing between the new macro services and if the new macro service needs some data I would be exposing it through an API or publishing event. The system should be backward compatible and ols and new flow can run together. I will add proper monitoring, logging and rollback plan.

### Users from different countries are using the applications but some users are from a specific locations are saying that they're facing slow response. How would you design the microservice system for low latency globally? 
 
I will first verify why these users were facing latency issue. It can happen that the server is far away from the user, the database region is different and maybe the external API are slow.  

To reduce the latency I will deploy services in multiple regions and if the business needs global traffic then it's better to use Geo routing or global load balancing so that the user request goes to the nearest region. 


For static data I would be using CDN and for frequently used data caching to the local server is the best option. 
For the database read it's preferred to use the read replicas in different regions. For writes multi region write would be giving you some concurrent issues so it's better to have a single write.  

I will be also monitoring the latency region wise so that we can clearly see which region is slow.

###  The microservice handles critical payment of financial data where failure is not acceptable. How will we make this service highly reliable and fault tolerant? 
The critical financial service should be designed to be highly reliable from the very beginning. 

Firstly I will be making multiple instances of the service so that one instance goes down another one will handle the traffic.  
Deploy across multiple zones if possible so that even if one zone has an issue the service will still work from another zone.   
Maintain proper database backup and replication as the data loss is not acceptable.  
The use of idempotent key for payment APIs it's very important so that the duplicate payment should not be saved into the db in case same request comes again.   
The retreat should be there with proper limits and timeouts. The retreat should not be blindly as it will create duplicate operations or extra load.     
The design you should be using circuit breaker for external dependency failures and a proper monitoring logging allowed auditing and reconciliation jobs. Reconciliation helps computer payment services between the system and the payment providers. 

In short I will be using multiple instances, multi zone deployment, database replication, idempotency key, timeouts, circuit breaker, audit logs, monitoring, reconciliation job to make the service reliable 

### Traffic suddenly became very high like in thousands and millions of requests coming in a very short period of time period how will you design a microservice of the system can handle properly?

Firstly the microservice should be stateless. Stateless meaning the service should not store user session or important state and set the server memory. If the service is stateless we can easily add more instances.  
The next step would be using horizontal scaling. It means that adding more services instances instead of making one server very big. 
I will put load balancer in the front so that the traffic gets distributed across all the instances. 
Auto scaling based on the CPU usage, memory, request count or queue size. 
For repeated data I would be using caching. 
For heavy or non urgent data at the asynchronous processes again going to be the top when using Kafka or any topic and making it even driven architecture. 
For database scaling I would be using good replicas, proper indexing, partitioning or sharding depends on the use cases. 
I will be adding a rate limit at the API gateway to protect the system from certain overload.

### In production one microservice suddenly starts using very high CPU after deployment. How will you debug this production performance issue? 
Firstly we need to verify that if this issue is happening after the latest deployment in that case I would be comparing the recent code changes and figure out what actually making this CPU usage. 

There might be some sort of infinite loops, heavy logic, long stream operations, unnecessary processing or maybe bad configuration. 
The next step is to see the traffic maybe the traffic is actually increased which is where the CPU is high.  
The application metric can give a lot of insights I will be seeing the API which is taking more CPU, which endpoint is getting called more and any background jobs is running.   

The application router logs for repetitive error. Sometimes continuous exceptions or retrace can also increase CPU.  
 The thread dumps and heap dumps are also going to be very useful. The thread dumps can show if threads are stuck mostly running.    
The next step is the database query. Sometimes the slow DB calls create thread blocking and then the system loads increases. If the issue is very serious then rolling back to the previous stable version is far more benefited and then find the root cause of the redeployed it.

### You need to support real time updates like chat messages, order stress updates, live notifications or alerts. How will you present the microservice? 
The first step is to design a real time update systems is either by using websocket or server side events depending on the requirement. In case needs to do a communication then websocket and in case if only the server will be pushing the server side events it will be helpful.
When any event happens then the messages will be received and responsive services will publish it to the Kafka. Then the notification service of the real time applications will consume that events. After consumption of the event it will be pushed to the update to the connected user through websocket. If the user is offline will store this message and notifications in the database and when the user comes online then pending notifications will be sent  
 
The application should make sure that the website connections are scalable. The use of sticky sessions or exchange storage like Redis and Kafka and push updates through websocket or SSE. 
### The application is calling a third party API for the payment KYC , SMS, e-mail and that API starts failing randomly. How will you protect your system from this failure ?

The first instance should be like not designing any system that can allow a third party API failure to bring the entire system. 
First I will be adding a timeout In case a third party doesn't respond at a fixed time the service shouldn't wait forever.   

Add the retry in limited range. For example like retrieve for two to three times only for temporary failures and also adding with an exponential backup so that the return doesn't happen in equal gap. 

The design pattern should be circuit breaker. In case a third party application is failing circuit breaker will stop calling it for some time so that it doesn't make any additional load. 

I will be adding fall back whenever possible like if SMS failed then it should be retried later through a queue, if e-mail fails it should be marked as pending and processed later, if the KYC fails it kept in pending and reconcile later.  

I will also monitor third-party error rate, timeout count and response time.

###  Before releasing any micro service to the production how will you test them properly so that individual services and complete user flow works properly ?

The microservice should be tested on multiple levels. 
 
Unit testing- to test the internal business logic of one service for example validation logic, calculation logic, service layer logic. 
Integration testing- it helps us test whether the service is working properly with database, Kafka, weddings, external APIs . 
Contract testing - it is important microservices one service depends on another service's API or external event structure. It will make sure that the producer and consumers are following the same contract.
End to end testing - it will test the complete user flow for example order create, process payment, update inventory, send notification. 
Testing scenarios - the way an application should react when the payment service down, Kafka message failure, timeout, duplicate request.  
Did you plan to the productions we need to 1st perform a performance testing and regression testing. 


### The todo - Optimistic locking and pessimistic locking - study and project and code.
### API gateway and traceID, centralised logs set up in an application.
### Only API gateway should be public. - Apigee or gateway?