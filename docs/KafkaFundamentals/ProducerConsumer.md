**Kafka Broker** - Server in Kafka storage layer. Stores topics/partitions.  
**Kafka Cluster** - Collection of multiple brokers. Every broker can act as a Bootstrap Server.  
Connect to one broker → connect to whole cluster.

**Security**  
- Encryption: SSL/TLS.  
- Authentication: SASL or SSL.  
- Authorization: ACLs.  
- Disabled by default.   

**APIs**  
- Consumer API → Subscribe and consume from one/more topics.  
- Streams API → Consume from input topics, process stream, produce to output topics. Easy DSL, low operational overhead.  
- Admin API → Create, delete, inspect, and manage topics, brokers, ACLs, and Kafka objects.



**REST Proxy**  
- RESTful HTTP service to interact with Kafka.  
- Uses Admin API internally.  
- Supports - Produce messages, Consume messages, Consumer groups, Cluster metadata inspection.  
- Integrates with Schema Registry.  
- Converts JSON ↔ Avro automatically.  
- Any language can use Kafka via HTTP + JSON.  

**Schema Registry**  
- Centralized schema repository for the topic data.  
- Manages and validates schemas.  
- Used for serialization/deserialization.

**Consumer Group**  
- Consumer Group = Set of consumers consuming from topics.  
- Configure using group.id.  
- To use the subscribe() and commit the consumer needs to be a part of consumer group.
- Use assign() instead of subscribe() to manually assign partitions and it does not need to be a part of group. In that case, consumer group coordination/rebalancing is not used.
  

**Partition Assignment**  
- Partitions are divided among consumers in the group.  
- One partition → only one consumer in group can consume it.  
- Example - 
  - 4 consumers, 1 partition → only 1 active consumer.  
  - 4 consumers, 8 partitions → each gets 2 partitions.  
- One consumer can read - 
  - Multiple partitions.  
  - Multiple partitions from same topic.

**Rebalancing** - New consumer joins or old leaves → partitions reassigned. Its Rebalance.

**Group Coordinator**  
- One broker acts as Group Coordinator.
- Manages - Group members and Partition assignments.
- Chosen from leaders of `_consumer_offsets` that stores committed offsets.

**Heartbeat** - Consumers send heartbeat periodically. No heartbeat → consumer removed. Rebalance triggered.

**Offset Management**  
- Initial Offset - 
  - Controlled using auto.offset.reset - earliest or latest.
- Offset Commit - 
  - Consumer reads message → commits offset.
  - If consumer crashes → next consumer starts from last committed offset.

**Auto Commit**  
- Default behavior = Automatic offset commit.
- Commit happens at regular interval.
- Config - 
  - `enable.auto.commit=true`  
  - `auto.commit.offset.interval.ms`
- Delivery Semantics - Auto commit gives at-least-once delivery.
- Duplicate Scenario - 
  - Messages after last commit are re-read after failure.
  - Message processed but offset not committed → read again.
- Data Loss Scenario - 
  - Auto commit happens before processing completes.
  - Crash after commit but before processing → data loss.
- Reduce Duplicates - Reduce auto-commit interval.

**Manual Commit**  
Set - `enable.auto.commit=false`    
**Synchronous Commit**  
- Blocking call until commit response returns.  
- Improve Throughput. 
  - Increase poll batch size using - `fetch.min.bytes`.  
  - Broker waits until - enough data available OR `fetch.max.wait.ms` expires.

**Asynchronous Commit**  
- Consumer sends commit request and continues immediately.
- Faster than sync commit.
- Issue -   
  - Failed commits are not retried automatically.
  - Commit ordering issue - Later offsets may already be committed. Retrying old commit can create duplicates.

# Kafka Consumer Configuration

| Config                                       | Purpose                                                                                              | Important Notes                                                                                                                                                                                                |
|----------------------------------------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `bootstrap.servers`                          | Initial broker list for connecting to Kafka cluster.                                                 | Consumer discovers full cluster from these brokers.                                                                                                                                                            |
| `client.id`                                  | Identifies the consumer client instance.                                                             | Used for logging, metrics, request tracing on broker side. Usually unique per consumer instance.                                                                                                               |
| `group.id`                                   | Consumer group identifier.                                                                           | Required for `subscribe()` and Kafka-managed offset commits. Not required with manual `assign()`.                                                                                                              |
| `session.timeout.ms`                         | Time coordinator waits before marking consumer dead.                                                 | Default ~10 sec. Longer timeout delays failure detection/rebalance. Network issues or long GC pause can trigger rebalance. Consumer normally sends LeaveGroup before shutdown.                                 |
| `heartbeat.interval.ms`                      | Interval for sending heartbeats to coordinator.                                                      | Default 3 sec. Must be lower than `session.timeout.ms`. Missing heartbeats triggers rebalance.                                                                                                                 |
| `max.poll.interval.ms`                       | Maximum allowed time between `poll()` calls.                                                         | Default 300 sec. If exceeded, consumer considered failed and rebalance happens.                                                                                                                                |
| `max.poll.records`                           | Max records returned in each `poll()`.                                                               | Tune batch size to avoid processing delays and `max.poll.interval.ms` timeout.                                                                                                                                 |
| `enable.auto.commit`                         | Enables automatic offset commits.                                                                    | Default `true`. Consumer commits offsets periodically.                                                                                                                                                         |
| `auto.commit.interval.ms`                    | Interval for auto offset commit.                                                                     | Default 5 sec. Smaller interval reduces duplicate window but increases commit overhead.                                                                                                                        |
| `auto.offset.reset`                          | Defines behavior when no committed offset exists or offset is invalid/out of range.                  | Options- `latest` (default), `earliest`, or reset based on configured `duration/timestamp`.                                                                                                                    |
| **Kafka Partition Assignment Configuration** |                                                                                                      |                                                                                                                                                                                                                |
| `partition.assignment.strategy`              | Defines how partitions are distributed among consumers in a group.                                   | All consumers in the same group must use the same strategy. Accepts comma-separated fully qualified assignor class names implementing `PartitionAssignor`.                                                     |
| `group.protocol=classic`                     | Enables classic consumer group protocol.                                                             | `partition.assignment.strategy` works only with `classic` protocol (default).                                                                                                                                  |
| `group.protocol=consumer`                    | New consumer group protocol.                                                                         | `partition.assignment.strategy` not supported.                                                                                                                                                                 |
| Assignment Strategies                        |                                                                                                      |
| `RangeAssignor` (Default) | Assigns partition of each topic across the consumers in the consumer group.                          | Simple and efficient. Good when partitions > consumers.                                                                                                                                                        | Uneven distribution if partitions not divisible by consumers. |
| `RoundRobinAssignor` | Assigns partitions one-by-one across all consumers.                                                  | Better balanced partition distribution.                                                                                                                                                                        | Can trigger more rebalances. |
| `StickyAssignor` | Tries to keep existing assignments during rebalance.                                                 | Minimizes partition movement and duplicate processing.                                                                                                                                                         | May not always balance evenly. |
| `CooperativeStickyAssignor` | Incremental rebalance version of StickyAssignor.                                                     | Less disruptive and lower rebalance impact.                                                                                                                                                                    | Requires Kafka client `2.4+` and compatible consumers. |
| `ConstrainedCooperativeStickyAssignor` | Advanced sticky assignor enables more incremental rebalancing, which can reduce the latency and resources required during the rebalance process. | Reduces rebalance impact, not the frequency of rebalances. For consumer groups where all members subscribe to the same set of topics, it provides the same benefits and is optimized for this common scenario. | Works for common-topic subscription pattern only. |


`kafka-consumer-groups` command line to view and manage consumers group.

**List consumer groups** - `bin/kafka-consumer-groups --bootstrap-server host:9092 --list`    

**Describe groups** - `bin/kafka-consumer-groups --bootstrap-server host:9092 --describe --group test-1234`  

**Reset offsets** - reset offsets by shifting forward or backward with shift-by or reset them to the beginning with `--to-earliest`.   
To reset the offsets back by 20 positions `bin/kafka-consumer-groups.sh --bootstrap-server host:9092 --group test-1234 --reset-offsets --shift-by -20 --topic test-metrics -execute --group test-1234`

All messages in a topic are on the same broker - ❎ A topic is split into partitions, and those partitions are distributed across multiple brokers

All messages in a partition are on the same broker - ✅ All messages in a partition are on the same broker. Replicas of the partition exist on other brokers, but the leader holds the active copy where producers write and consumers read.

All messages with the same key will be on the same broker. ✅ Messages with the same key are guaranteed to be in the same partition, and since a partition is hosted on a single broker, they will be on the same broker. 

The more partitions a topic has, the better. ❎ It creates overhead.

Suppose there is a message in our Kafka cluster about my breakfast purchase of $12.73. Consumer c0 has consumed it to process the charge. Could consumer c7 consume this same message this afternoon? - Yes
