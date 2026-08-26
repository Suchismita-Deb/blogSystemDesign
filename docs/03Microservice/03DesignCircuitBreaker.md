### Circuit Breaker Patter using Resilience4j.

Order Service calling Inventory Service.

The inventory service down and there is retry mechanism and when its down then repeated call is making more load and it will consume CPU, thread, Network connections. When the order service knows that the inventory service is down then it will not send the record.

There is a utility added in the order service. It will see in case the inventory service is active and in case 50% of the response is not successful then it will inform that inventory service is down and it will ask the order service to handle it.


Circuit Breaker stops sending request to the unhealthy service after repeated failures.

There are mainly 3 states in Circuit Breaker - OPEN, CLOSED, HALF_OPEN

CLOSED - All request are sent to the service and it is healthy. Initially it will be closed. The circuit breaker will records things like successful calls, failed calls, slow calls, failure rates.  
Example 10 calls and 7 success and 3 failures. The failure rate is 30%. In case the configured rate is 50% then the circuit remains closed.

OPEN - When the failure rate is more than the configured rate then the circuit breaker will open and it will not send any request to the service. It will return a fallback response. The circuit breaker will remain open for a configured time. After that it will go to HALF_OPEN state.
```java
failureRateThreshold: 50
minimumNumberOfCalls: 10
```
In case 7 failures in the last 10 calls then the request will not go to the inventory service and it will prevent the cascading failures.

HALF_OPEN - The circuit does not stays open forever. The `waitDurationInOpenState: 10s` meaning after 10 sec it will allow a limited number of test calls example `permittedNumberOfCallsInHalfOpenState: 3` to the service. If the test calls are successful then it will go to CLOSED state and if it fails then it will go to OPEN state again.

The failure percentage is coming from the sliding window of the calls like the `slidingWindowSize: 10`.

When the configured failure threshold is exceeded the circuit transitions to OPEN and prevents further calls to the failing downstream, dependency for the configured wait duration. It prevents the cascading failures.
In the Order service we will put the circuit breaker before calling the service.
```java
@CircuitBreaker(name = "inventoryServiceCircuitBreaker", fallbackMethod = "circuitBreakerFallbackMethod")
public Inventory getInventory(Long productId) {
    System.out.println("Calling Inventory Service for productId: " + productId);
    return inventoryClient.getInventory(productId);
}
public Inventory circuitBreakerFallbackMethod(Long productId, Throwable throwable){
    System.out.println("Fallback Method Called for productId: " + productId);
    return new Inventory(productId.toString(),0);
}

```

In the YAML we have to set the configurations.
```yaml
resilience4j:
  circuitbreaker:
    instances:
      inventoryServiceCircuitBreaker:

        # Sliding Window Configuration
        sliding-window-type: COUNT_BASED
        sliding-window-size: 6
        minimum-number-of-calls: 1 # It is the minimum number of calls until it the circuit breaker will not calculate the failures.

        # Failure Threshold
        failure-rate-threshold: 50
        # In case the minimum-number-calls is set to 2 and the failure rate is 50 then one request fail will trigger error.

        # Open State Configuration
        wait-duration-in-open-state: 10s

        # Half Open Configuration
        permitted-number-of-calls-in-half-open-state: 3
        max-wait-duration-in-half-open-state: 5s
        automatic-transition-from-open-to-half-open-enabled: true

        record-exceptions:
          - java.lang.RuntimeException
          - java.io.IOException
          - org.springframework.web.client.HttpServerErrorException

        ignore-exceptions:
            - java.lang.IllegalArgumentException
```