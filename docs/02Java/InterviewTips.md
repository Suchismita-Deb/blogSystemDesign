### Explain the project you are working on?

Its a very imp point and it will set the direction of the discussion. 
Time around 3 mins but hit the spot you want to highlight. 
In practical I have working with microservice and kafka dealing with 100M message in a week retention with 8 consumers and connectors.
I have working in another project like the labs report will be gereated in the dashboard and using the cqrs will make the microservice and generate the pdf report.

The role is highlighting Kafka, project 1 to discuss.


The point is I will decide what should be explored deeper by how much energy we put into each part when I explain. Weaker part flat one sentence but the stronger part gets more specifics and get mentioned last(recency bias - the next question will follow up the last part).
A draft looks like - One project I can go deep on is an order platform built as event driven microservice with Kafka as backbone between services.  

The core flow - When an order event fires it publishes a topic partitioned by the entity Id to preserve per-entity Ordering and several services - inventory, pricing, notification and each run their own consumer group and maintain their own view of the data instead of hitting the shared db.
I am primarily responsible for the maintaining the microservice and the consumer pipeline. The handling of data, validation, pushing processed data to the downstream service using the connector to give them the data format. Payload is Avro encoded as we are using the Confluent Cloud its easy to work as its a service provider and we are not taking care of the inhouse server.



I would like to give an instance of the case that I have faced in the application - We hit an intermittent deserialization failure on the consumer side. The message key is String and not like the Avro-encoded value and teh deserilaizer set up did not handle the mismatch cleanly so the message failed silently. Across a 12 partitioned topic the failure was not uniform so I narrowed it down to the partition by partition and it turned out the issue lies in the partition 3 by extracting the message directly and conparing the key encoding across partitions.
When identified the issue the fix was simple like key and value deserializer split but the main part was finding the root cause and narrowing to the partition.

There are many source and it was made the key as Avro and the source provided the key as String so teh downstream did not get the data properly in the table. I believe the issue might be the timeline for the deployment so when the changes are deployed the specific producer did not deployed the changes so for say 15 mins all data came as bad data with a different key and the consumer did not get good key to load.