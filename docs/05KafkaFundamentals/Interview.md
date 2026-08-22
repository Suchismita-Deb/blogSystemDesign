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