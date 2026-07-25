<div class="quiz-box">
<b>Sample tag.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
</details>
</div>

<div class="quiz-box">
<b>What are the different memory present in Java.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Java memory is managed by JVM and it has heap for objects and stack for each thread methods and calls for local variable. Method area or meta space for class metadata and static variable. PC (Program Counter) register for the current instructions per thread and native method stack for JNI calls.
</details>
</div>

<div class="quiz-box">
<b> The production of freezes intermittently under load how would you determine whether it's a deadlock a long GC pause or a throat starvation ? </b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Take a thread dump first. The deadlock show up as a clear cyclic logs. Verify the GCS logs for long post in case threads are in long pause.  
If there's a real life but just waiting on the pool then that's thread starvation. The notice part is thread poll soze and queue length.
</details>
</div>

<div class="quiz-box">
<b> A counter incremented by multiple threads inside a synchronized method is still producing wrong totals what are the possible causes ? </b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
The solution involves for us to save the whole method is entirely synchronized on just part of it.  
The next thing to see if multiple objects are being used as locks instead of one shared locsk.  
In case the counter is itself has a non atomic Read modify write happening outside the synchronized block 

</details>
</div>


<div class="quiz-box">
<b>How can a application leak memory even though the JVM has garbage collection, give an example </b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Elementary difference happens when object was still getting referenced somewhere so GC Will not be able to collect them even the app is not using it anywhere. Example - static collections keep on growing.

</details>
</div>

<div class="quiz-box">
<b>How to default methods let an interface evolve without breaking existing implementing classes ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Adding a method into the interface everyone has to implement it. 
Default method let up add a method wth a body directly in the uinterface.
The entire implementing classes compiled and worked without being forced override.   
</details>
</div>

<div class="quiz-box">
<b>How do you monitor application connections and identify failing connections ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
We should use Actuator health endpoint metric for connection pool stat and monitoring tools like Prometheues, Grafana for dashboard and alerts. 
Application and pool logs also shows the failed or the timeout connections.
There should be some sort of alerts and pool usage and failed connection.
</details>
</div>

<div class="quiz-box">
<b>How would you handle the duplicate messages sent by the Kafka producer ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

The first thing to enable idempotent key setting for the retries dont create duplicates and the consumer side make it idempotent.
</details>
</div>

<div class="quiz-box">
<b>How would you troubleshoot Kafka consumer that is unable to consume messages?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
First see the consumer group status and the lag and see if the consumer is part of the group.

In case stuck in rebalancing then check the network connectivity to the broker and the topic. 
</details>
</div>

<div class="quiz-box">
<b>How would you handle a Kafka consumer can that continuously fails while processing messages ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
We should use retry with backup for transient error. In case it is failing then send the letter to the dead letter review topic with the proper error message to debug and understand the issue and alert set up when message going to the dead letter queue topic.
</details>
</div>

<div class="quiz-box">
<b>What is Springwood Actuator and how would you use it to monitor a live app's health and connections ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
The actuator exposes production ready endpoints like health, metrics, info, env, beans, mappings, threaddump, loggers, httptrace and many more. We can see database and connection pool health, memory usage and custom health indicators hooked up to the Prometheus.
</details>
</div>

<div class="quiz-box">
<b>How does Spring Boot auto configuration work internally like the @ConditionalOnClass and @ConditionalOnMissingBean?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Spring Boot scan auto configuration classes listed on the auto configurations.import file. Each class activates when the condition matches.
Example ConsitionalOnClass in case a dependency is on the class path or @ConditionalOnMissingBean in case the bean is not defined.
</details>
</div>

<div class="quiz-box">
<b>The Springwood service was throwing connection-po0l-exhausted errors in production so how would you solve this pool issue ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
The first step to see the active versus the idle connection in the pool matrix. The names of the connections that are not relased like long running query. The pool size ad the load and the slow query logs.

</details>
</div>


<div class="quiz-box">
<b>Any functional differences present between @Components, @Service and @Repository or its purely semantic ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
They are functionally same and its the semantics. The @Repository enables automatic exception translation converting db exceptions to spring data exceptions.
</details>
</div>


<div class="quiz-box">
<b>A teammate wants no sequel for scalability for a service that needs a strong consistency and joins how do you make the call ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
</details>
</div>

<div class="quiz-box">
<b>A legacy partner only supports SOAP but the team standard is REST how do you avoid duplicating logics ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
</details>
</div>

<div class="quiz-box">
<b>Your team's deploying a springboard services to EC2 for the first time what would you do as pre production setup ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
</details>
</div>