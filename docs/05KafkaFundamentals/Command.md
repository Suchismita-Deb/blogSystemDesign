Command to create a topic.

```bash
C:\kafka_2.12-3.9.1> bin\windows\kafka-topics.bat --create \
--topic testing \
--bootstrap-server localhost:9092 \
--partitions 1 \
--replication-factor 1
```

List of all topic or describe topic.
```bash

C:\kafka_2.12-3.9.1>bin\windows\kafka-topics.bat --bootstrap-server localhost:9092 --describe --topic demo_topic_1

C:\kafka_2.12-3.9.1> bin\windows\kafka-topics.bat --list --bootstrap-server localhost:9092
```

Produce message.
```bash
C:\kafka_2.12-3.9.1>bin\windows\kafka-console-producer.bat --topic demo_topic_1 --bootstrap-server localhost:9092
> Hello
> World
```
Consume message.
```bash
C:\kafka_2.12-3.9.1>bin\windows\kafka-console-consumer.bat \
--topic demo_topic_1 \
--bootstrap-server localhost:9092 \
--from-beginning
```
Add partition after the server `--partition 1`

The default kafka-console producer and consumer take the key as null. It needs to specify the key and value to print it.

```bash
ka-console-producer \
  --bootstrap-server kafka:9092 \
  --topic testing \
  --property parse.key=true \
  --property key.separator=,

O/p.

> 1,my first record
> 2,another record
> 3,Kafka is cool
```

Kafka stores all metadata in zookeeper. 
```bash
Connect to zookeeper 

C:\kafka_2.12-3.9.1>bin\windows\zookeeper-shell.bat localhost:2181

To see the directory - ls 
The folder like broker, config, consumers.

ls /brokers

The folder and id will be there.
```
Java config in the Producer.java file - `solution/java-producer/src/main/java/clients/Producer.java`
```java
// Setting the bootstrap server.
settings.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "kafka:9092");


```

```java
// Configure the bootstrap server
settings.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");

// Populate the message object
final ProducerRecord<String, String> record = 
    new ProducerRecord<>(KAFKA_TOPIC, key, value);

// Write the lat/long position to a Kafka topic.

// Print the key and value in callback lambda.
producer.send(record, (md, e) -> {
    if (e != null) {
        e.printStackTrace();
    } else {
        System.out.printf(
            "Sent Key:%s Value:%s Topic:%s Partition:%d Offset:%d%n",
            key,
            value,
            md.topic(),
            md.partition(),
            md.offset()
        );
    }
});
```
The data of `batch.size` send after `linger.ms` the consumer will send the record.  
The consumer running and the command to see the consumer group and the offset of the consumer group. 

```bash
kafka-consumer-groups `
  --bootstrap-server kafka:9092 `
  --describe `
  --group java-consumer
```
`CURRENT-OFFSET` is the last committed offset from your consumers.   `LOG-END-OFFSET` is the last offset in each partition

The `auto.offset.reset` only applies when there is no committed offset for the consumer group in the first run.  
On the second run, the consumer finds its previously committed offsets in the __consumer_offsets topic.  
The offset exists and it will resume from the position.  
To replay from start → delete/reset offsets, or use a new group id.
The `kafka-consumer-groups.sh --reset-offsets --to-earliest` command is the standard way to force your consumer group to start again from the beginning of each partition.  
An instance in a consumer group sends its offset commits and fetches to a group coordinator broker. The group coordinators read from and write to special compacted Kafka topic named __consumer_offsets.


Consumer command - `fetch.min.bytes` and `fetch.max.wait.ms`.
```java
settings.put(ConsumerConfig.FETCH_MAX_WAIT_MS_CONFIG, "5000");
settings.put(ConsumerConfig.FETCH_MIN_BYTES_CONFIG, "5000000");


// Poll fpr records. The consumer will wait up to 5 seconds to fetch at least 5MB of data before returning the records. If 5MB of data is not available within 5 seconds, it will return whatever data is available.
final ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
System.out.printf("Key:%s Value:%s [partition %s]\n", record.key(), record.value(), record.partition());
```

The `consumer_offset` topic get the offset commit details from the group coordinator and store the offet details.
We grep the topic to get the data.
```bash
kafka-console-consumer --bootstrap-server kafka:9092 \
--topic __consumer_offsets \
--formatter "kafka.coordinator.group.GroupMetadataManager$OffsetsMessageFormatter" \
| grep 'driver-positions'
```
OffsetsMessageFormatter - Formatter that decodes the binary offset commit message to human readable form.

grep 'driver-positions' → Filters the output to only show records in __consumer_offsets that belong to the consumer group named driver-positions

The output will show the consumer offset in the partitions.



```
driver-positions, topic: my-topic, partition: 0, offset: 12345
driver-positions, topic: my-topic, partition: 1, offset: 67890
```
When we make a new consumer group id it will get the data from the beginning.

|Property| Role                                                                           |
|---|--------------------------------------------------------------------------------|
|`fetch.min.bytes`| Minimum batch size the broker should return for a fetch request.               |
|`fetch.max.wait.ms`| Broker won’t wait longer than this; if min.bytes isn’t reached, it still replies after this time. |


Producer write to the Avro serialized topic. The code will communicate with Schema registry and retrieve schema and serialize the data in Avro format.
Avro file .avsc and the plugin gradle.plugin.avro include generateAvroJava to generate class from the avro schema. The generated class will be used in the producer code to create the record and send it to the topic.

```java
// Avro file user.avsc
{
    "type": "record",
    "name": "User",
    "namespace": "example.avro",
    "fields": [
        {"name": "id", "type": "int"},
        {"name": "name", "type": "string"},
        {"name": "email", "type": "string"}
    ]
}

// The json equivalent record.
{
    "id": 1,
    "name": "Suchismita",
    "email": "suchismita@example.com"
}
```

```java
// Kafka with schema registry.
public class AvroProducer {
    public static void main(String[] args) throws Exception {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("key.serializer", KafkaAvroSerializer.class.getName());
        props.put("value.serializer", KafkaAvroSerializer.class.getName());
        props.put("schema.registry.url", "http://localhost:8081");

        Producer<Object, Object> producer = new KafkaProducer<>(props);

        String schemaString = "{\"type\":\"record\",\"name\":\"User\",\"fields\":["
                + "{\"name\":\"id\",\"type\":\"int\"},"
                + "{\"name\":\"name\",\"type\":\"string\"},"
                + "{\"name\":\"email\",\"type\":\"string\"}]}";

        Schema schema = new Schema.Parser().parse(schemaString);

        GenericRecord user = new GenericData.Record(schema);
        user.put("id", 1);
        user.put("name", "Suchismita");
        user.put("email", "suchismita@example.com");

        ProducerRecord<Object, Object> record =
                new ProducerRecord<>("users-topic", user);

        producer.send(record);
        producer.flush();
        producer.close();
    }
}
```
How the schema registry works.

Define the schema `user.avsc` - Producer sends the message **KafkaAvroSerializer** contacts the Schema Registry at the url - The schema is stored in the schema registry and assigned to an ID - The message serialized with Avro - Consumer use the KafkaAvroDeserializer then queries the schema registry and get the message.