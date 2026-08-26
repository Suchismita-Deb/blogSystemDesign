Idempotency.

Kafka gives order like Order-123 and add 100 to the db and the db updated then commit the offset the consumer restarts and the message is again processed in the db and add 100. 

The idempotency meaning same request will not make update the db again. The eventId and idempotency store and when the same id exists them it will not be processed.

Kafka key is not the idempotency key we put the value in the condition to see data present in the db.
```java
@Transactional
public void process(OrderEvent event) {

    if (processedEventRepository.existsById(event.getEventId())) {
        return;
    }

    saveOrder(event);

    processedEventRepository.save(
        new ProcessedEvent(event.getEventId())
    );
}
```

### There is a Kafka topic receiving 1M events per minute and the consumer application is unable to run at the same speed. What would you investigate first and how would you scale the Kafka consumer side to handle the increased load?

The consumer need to scale so the main part to see like the number of partition and technically one consumer can consume one partition. 

The first part t identify in case kafka is the bottleneck meaning partition count, producer throughput, broker health, partition distribution and consumer lag by partition.      
In case kafka good then the consumer not able to take the load in that case in increasing multiple consumer might help for short time. Each consumer will consume each partition. A single spring boot application will have multiple consumers. The example say one db call 10 ms then 1M per minute it will give lag. In case its the db issue then db side needs to be updated like consumer - deserialize - business logic - db calls - commit. It will make the db work more.
Producer batching can improve the producer or the broker efficiency but the primary focus for the consumer lag would be like partitioning, consumer parallelism, consumer processing time and downstream bottleneck.  

