### CLI command to manage Kafka ACLs.

Kafka Access Control List - `kafka-acls.sh` It is used to add, remove and list ACLs for ropic, consumer group, clusters and resources.

List all ACLs
```shell
kafka-acls.sh \
  --bootstrap-server localhost:9092 \
  --list
```

User read access

```shell
kafka-acls.sh \
  --bootstrap-server localhost:9092 \
  --add \
  --allow-principal User:alice \
  --operation Read \
  --topic orders
```

Producer permission
```shell
kafka-acls.sh \
  --bootstrap-server localhost:9092 \
  --add \
  --allow-principal User:producer1 \
  --operation Write \
  --topic orders
```
Consumer Permission
```shell
kafka-acls.sh \
  --bootstrap-server localhost:9092 \
  --add \
  --allow-principal User:consumer1 \
  --operation Read \
  --topic orders \
  --group order-group
```

Remove ACLs
```shell
kafka-acls.sh \
  --bootstrap-server localhost:9092 \
  --remove \
  --allow-principal User:alice \
  --operation Read \
  --topic orders
```


### What is plugin discovery in Kafka Connect, and how is it controlled?

Plugin discovery is the process of finding and loading plugin classes in Kafka Connect. It is controlled by the "plugin.discovery" worker configuration. The "service_load" strategy is the fastest but should be used with caution to verify plugin compatibility due to potential issues during worker startup.

Plugin discovery is the name for the strategy which the Connect worker uses to find plugin classes and make them accessible to configure and run in connectors. This is controlled by the plugin.discovery (https://kafka.apache.org/documentation/#connectconfigs_plugin.discovery) worker configuration, and has a significant impact on worker startup time. service_load is the fastest strategy, but care should be taken to verify that plugins are compatible before setting this configuration to service_load.

### Producer configuration used to increase the chance of batching messages.

max.request.size=1048576   
acks=all                  
linger.ms=10              
batch.size=32768          

Answer - linger.

Batch strategy in kafka producer will batch messages going to the same partition. The most imp step to optimize throughput - tune the producer batching to increase the batch size and the time spent waiting for the batch to populate with messages. Larger batch sizes result in fewer requests, which reduces load on producers and the broker CPU overhead to process each request.

linger.ms is the number of milliseconds a producer is willing to wait before sending a batch out (defaults to 0). By introducing some lag, we increase the changes of messages being sent together in a batch. If the batch is full (batch.size) before the end of linger.ms period, it will be sent to Kafka right away.

linger.ms is the primary producer configuration specifically designed to increase the chance of batching messages by making the producer wait for a short period before sending records, allowing more messages to accumulate in the batch.

batch.size controls the maximum size of a batch, but it does not actively increase the chances of batching.

The producer side configuration.

```properties
# Reliability
acks=all
enable.idempotence=true
retries=Integer.MAX_VALUE
max.in.flight.requests.per.connection=5
# Performance
batch.size=32768
linger.ms=10
compression.type=snappy
# Message Size
max.request.size=1048576
buffer.memory=33554432
```
The consumer side configuration.
```properties
group.id
auto.offset.reset
enable.auto.commit
max.poll.records
fetch.min.bytes
fetch.max.bytes
session.timeout.ms
heartbeat.interval.ms
key.deserializer
value.deserializer
```

### In Kafka Streams, what determines the maximum parallelism at which an application may run?

The total number of messages in the input topic.  

The number of state stores used in the application.  

The number of Kafka brokers in the cluster.  

The maximum number of partitions of the input topic(s) the application is reading from.

The maximum number of partitions of the input topic(s) the application is reading from.

The maximum parallelism in a Kafka Streams application is determined by the maximum number of partitions of the input topic(s) the application is reading from in its processing topology. The number of input partitions sets the upper bound on the application's parallelism, as it determines how many stream tasks Kafka Streams creates for the application.

For instance, if an application is reading from an input topic with 5 partitions, the maximum parallelism of the application is 5. Attempting to run more than 5 instances of the application will result in excess instances being idle. Understanding the input topic's partitioning is crucial for optimizing the parallelism of a Kafka Streams application.

### A company has a Kafka cluster with 4 brokers. For a new streaming application, the company wants to create a production topic with two partitions that is resilient to one broker failure and one broker maintenance. What replication factor will the company specify while creating the topic?

3
Kafka replicates the log for each topic's partitions across a configurable number of servers (you can set this replication factor on a topic-by-topic basis). This allows automatic failover to these replicas when a server in the cluster fails so messages remain available in the presence of failures

The total number of replicas including the leader constitute the replication factor.  
The Topic replication factor should always be greater than 1, otherwise it doesn't provide resilience to a single server failure. A replication factor of 2 is also not enough, because this does not provide resilience to a server failure and one server being down for maintenance. A replication factor of 5 is not possible, as the company only have 4 brokers in the Kafka cluster.

### Which acknowledgment configuration should be used for a producer to maximize performance?

acks = 0

In Kafka, producers create and write messages to topic partitions and producers can choose to receive acknowledgement of data writes, which can be configured using the acks property. It is the number of acknowledgments the producer requires the leader to have received before considering a request complete. This controls the durability of records that are sent.

If acks=0 then the producer will not wait for any acknowledgment from the server at all. No guarantee can be made that the server has received the record in this case. However, because the producer is not waiting for any response from the server, it can send messages as fast as the network will support, so this setting can be used to achieve very high throughput.

### A company created a production topic named prod_topic. The retention period for prod_topic should be 10 days. How could you accomplish this in the most efficient way?

Set the topic configuration retention.ms=864000000.

The property retention.ms can be configured at topic level while creating topics or by altering topics. It shouldn't be set at the broker level (log.retention.ms) as this would impact all the topics in the cluster, not just the one we are interested in.

The retention.ms configuration controls the maximum time in milliseconds we will retain a log before we will discard old log segments to free up space if we are using the "delete" retention policy. This represents an SLA on how soon consumers must read their data. If set to -1, no time limit is applied. The default value is 604800000 (7 days).

### In a consumer group application, what internal consumer mechanism can be used to detect a data processing issue?

Poll Thread

Consumers in a group talk to a consumer group coordinator. To detect consumers that have processing issues or are down, there is a poll and a heartbeat mechanism.

Heartbeat thread - Heartbeats are sent periodically to the broker. If no heartbeat is sent during that period, the consumer is considered dead.

Poll thread - This is particularly relevant for Big Data frameworks like Spark in case the processing takes a lot of time. This mechanism detects data processing issues.

### Which API can be used to shutdown a consumer from another thread?

flush()  
shutdown()  
close()   
wakeup()

wakeup()      

Multithreading is the ability of a central processing unit (CPU) to provide multiple threads of execution concurrently, supported by the operating system. In situations where the work can be divided into smaller units, which can be run in parallel, without negative effects on data consistency, multithreading can be used to improve application performance.

When implementing a multi-threaded consumer architecture, it is important to note that the Kafka consumer is not thread safe. Multi-threaded access must be properly synchronized, which can be tricky. This is why the single-threaded model is commonly used.

The only exception to this rule is wakeup(), which can safely be used from an external thread to interrupt an active operation. In this case, a WakeupException will be thrown from the thread blocking on the operation. This can be used to shutdown the consumer from another thread.