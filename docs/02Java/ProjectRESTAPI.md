HTTP status code and basics. Claude Learning.

The things that separates a Senior Engineer - Rate limiting, Pagination, Versioning, Idempotency, Circuit Breaker, HATEOAS, OpenAPI, API Gateway.

What RESTful means?

Most people think REST is JSON over HTTP with clean URLs. The main thing is the client-server, stateless, cacheable, layered system, uniform interface, code on demand.  
The stateless meaning - server holds zero session state between calls - auth and context must travel as token not a server session. Cacheable - server can mark responses as cacheable or not. Layered system - client does not know if it is talking to the end server or a proxy. Uniform interface - standard HTTP verbs, status codes, and media types. Code on demand - server can send code to the client to execute.


Full REST (Richardson Maturity Model) - Level 0 - single endpoint, Level 1 - multiple endpoints, Level 2 - HTTP verbs, Level 3 - HATEOAS.
The level 3 need HATEOAS - Hypermedia as the engine of application state. The server provides links to the client to navigate the API. The client does not need to hardcode URLs. Almost no production API is level 3 and mostly they are level 2 (resource + verbs + no hypermedia). Its not full REST.

Idempotency.

Calling an operations n times has the same effect as calling it once. In HTTP spec - GET, PUT, DELETE are idempotent. POST is not idempotent. Patch is depending on semantics. (Patch {status:"shipped") is idempotent and Patch {increment:1} is not idempotent).


client calls POST /payments network time out ans wait for the response. Server processed it and moved the money. Client retries - double charge. The solution is to make the POST idempotent by providing a unique key in the request header. The server will check if the key is already processed and return the same response instead of processing it again.

Idempotency-key pattern.
```java
@PostMapping("/payments")
public ResponseEntity<PaymentResponse> createPayment(@RequestHeader("Idempotency-Key") String idempotencyKey,        
                                                     @RequestBody PaymentRequest request) {
    String resultKey = "idem:result:" + idempotencyKey;    
    String lockKey = "idem:lock:" + idempotencyKey;
    // fast path: already completed, return stored response, don't reprocess    
    String cached = redis.opsForValue().get(resultKey);    
    if (cached != null) 
        return ResponseEntity.ok(deserialize(cached));
    // atomic check-and-set — THIS is where the real race is handled    
    Boolean acquired = redis.opsForValue().setIfAbsent(lockKey, "processing", Duration.ofSeconds(30));    
    if (Boolean.FALSE.equals(acquired)) {        
        throw new ConflictException("Request already in progress");    
    }
    try {        
        PaymentResponse response = paymentService.process(request);        
        redis.opsForValue().set(resultKey, serialize(response), Duration.ofHours(24));        
        return ResponseEntity.ok(response);    
    } finally {        
        redis.delete(lockKey);    
    }
}
```