### Kafka Fundamentals
- **Event Streaming System:** Kafka is a distributed event streaming platform that facilitates the publishing and subscribing of records (events) in a fault-tolerant and durable manner. It is designed to handle high-throughput, low-latency processing of real-time data feeds.

### Kafka Architecture
- **Brokers:** Kafka is run as a cluster on one or more servers called brokers. These brokers can be virtual machines or physical servers and are responsible for maintaining published data.
- **Topics and Partitions:** Topics are the categories or feeds to which records are published. Topics are divided into partitions to parallelize data and allow the system to scale horizontally. Partitions are distributed across different brokers.
- **Replication:** Partitions are replicated across multiple brokers to ensure fault tolerance. Each partition has one leader and multiple follower replicas. All read and write requests for a partition go through the leader, and followers replicate the leader's data.

![image](https://github.com/user-attachments/assets/0969537a-7bf4-4ed0-b468-19cac609722f)


### Kafka Producers and Consumers
- **Producers:** Producers publish records to Kafka topics. They can specify a key to influence the partitioning strategy.
- **Partition Strategy:** If a key is not provided, Kafka uses a round-robin strategy to distribute messages evenly across partitions. Kafka uses a default partitioner that employs a straightforward hash-based partitioning strategy when a key is provided. This partitioning strategy uses the hash of the key modulo the number of partitions to determine the partition to which a message should be sent. This ensures that all messages with the same key are sent to the same partition, which is crucial for maintaining the order of messages with the same key. Kafka also allows setting up custom partition stategy.
- **Consumers:** Consumers read records from topics. They can be organized into consumer groups, where each consumer within the group reads from exclusive partitions of the topic to ensure load is balanced across consumers.

### Kafka Data and Storage
- **Records:** A Kafka record consists of a key, value, timestamp, and optional headers. The key is used for partitioning and log compaction.
- **Disk Storage:** Kafka stores records on disk in an append-only log structure, which allows for efficient writes and reads. Each record within a partition is assigned a unique, sequential offset.
- **Segments:** Partitions are divided into segments, which are files stored on the disk. Segments help Kafka manage storage by enabling deletion or compaction of old data.

### Kafka Cluster Management
- **Zookeeper:** Kafka has historically used Zookeeper for managing the cluster and performing leader election for partitions. However, Kafka is moving towards removing Zookeeper in favor of its own internal consensus mechanism (KRaft).
- **ACLs:** Kafka uses Access Control Lists (ACLs) to control access to resources within the cluster.

### Kafka Durability and Fault Tolerance
- **Replication Factor:** The replication factor determines the number of copies of each partition. Kafka ensures that each partition's data is replicated to a configurable number of brokers.
- **Leader and Follower:** Each partition has one leader broker and multiple follower brokers. The leader handles all read and write requests for the partition, while followers replicate the leader's log.
- **Failover:** In the event of a leader broker failure, one of the follower brokers is promoted to be the new leader.

### Kafka Consumer Offsets
- **Consumer Offsets:** Kafka tracks the offset of the last record consumed by each consumer group in a special __consumer_offsets topic. This allows consumers to resume from where they left off in case of failure.

### Kafka Producer Guarantees
- **Acknowledgment Levels:** Producers can request different levels of acknowledgment from the broker to confirm message persistence: `acks=0` (no acknowledgment), `acks=1` (leader acknowledgment), and `acks=all` (full acknowledgment from all in-sync replicas).

### Kafka Log Compaction
- **Log Compaction:** Kafka supports log compaction on topics, which ensures that the log contains only the last known value for each key. This is useful for restoring state after a failure or for maintaining a changelog of state changes.

### Kafka Message Retention
- **Retention Policy:** Kafka does not delete messages after they are consumed. Instead, messages are retained based on a configurable retention policy, which can be time-based or size-based. This allows for message replay.

### Kafka Client Libraries
- **Client Libraries:** Kafka provides a variety of client libraries for different programming languages, which include APIs for producers, consumers, streams, and connectors.

### Kafka Delivery Guarantees
- **Exactly-Once Semantics:** Kafka offers exactly-once delivery semantics, ensuring that records are neither lost nor seen more than once, despite failures. This is achieved through idempotent producers, transactional writes, and careful management of consumer offsets.
- An idempotent producer can safely retry sending messages without the risk of duplicating them. This is achieved by assigning a unique sequence number to each message. The idempotence is enabled by setting the `enable.idempotence` configuration to true in the producer.
- Transaction writes, Kafka supports transactions that allow producers to send a batch of messages atomically to multiple partitions across different topics. Producer must be configrued with unique `transaction id`.
- Consumer offsets ensure no message is read twice by the same consumer.

Certainly! Here's a more structured format for the additional Kafka topics that can enhance your fundamental understanding:

### Kafka Connect
- **Integration Framework:** Kafka Connect is a scalable and reliable tool for streaming data between Kafka and other systems, such as databases and data warehouses.
- **Connectors:** It provides a large number of connectors for various common data sources and sinks ex: SQL, simplifying the integration process.

### Kafka Streams
- **Stream Processing Library:** Kafka Streams is a lightweight library for building real-time applications and microservices that process streams of data from Kafka topics.
- **Stateful Processing:** It supports complex transformations, aggregations, joins, and windowing operations on streaming data.

### Kafka Security
- **Authentication:** Kafka supports various authentication mechanisms, including SASL/PLAIN, SASL/SCRAM, and SASL/Kerberos.
- **Authorization:** Access control lists (ACLs) are used to grant or deny actions on Kafka resources for users and applications.
- **Encryption:** SSL/TLS can be used to encrypt data in transit between Kafka clients and brokers.

### Kafka Monitoring and Operations
- **Performance Metrics:** Kafka exposes a wide range of metrics that can be monitored using tools like JMX, Prometheus, and Grafana.
- **Logging:** Kafka brokers, producers, and consumers generate detailed logs that can be used for troubleshooting and auditing.

### Kafka Administration
- **Cluster Management:** Administrators can manage topics, partitions, replication factors, and broker configurations using Kafka's command-line tools and APIs.
- **Maintenance:** Routine tasks include balancing partitions across brokers, performing rolling updates, and ensuring cluster health.

### Schema Management
- **Schema Evolution:** Tools like Confluent Schema Registry provide a centralized repository for schema management, ensuring compatibility as schemas evolve over time.
- **Data Serialization:** Schema management is closely tied to data serialization formats like Avro, JSON Schema, and Protobuf.

### Kafka Ecosystem and Integrations
- **Complementary Technologies:** Kafka is often used in conjunction with big data platforms, stream processing engines (e.g., Apache Flink), and data lake solutions.
- **Third-Party Tools:** The Kafka ecosystem includes a variety of third-party tools for enhanced monitoring, management, and integration.

### Kafka Best Practices
- **Topic Design:** Best practices for naming topics, choosing the right number of partitions, and deciding on retention policies.
- **Configuration Tuning:** Optimizing producer and consumer configurations for throughput, latency, and durability based on specific use cases.
