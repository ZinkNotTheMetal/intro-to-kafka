# Event Driven Architecture (with Kafka and C#)

A software architecture pattern promoting the production, detection, consumption of, and reaction to **events**

## Definitions

**Event Sourcing** - An architectual style or approach to maintaining an application's state by capturing all changes as a sequence of time-ordered, immutable events.

**Broker** - An event broker is middleware software, appliance or SaaS used to transmit events between event producers and consumers in a publish-subscribe pattern. (in this case Apache Kafka)

**Message** - A basic unit of communication (object, string, number)

**Command** - initiated by a user or technical component, which asks a system to *perform* an action

**Event** - initiated by the system to express the fact that something *has* happened

* Producer - The person or action that is creating the event

* Consumer - also known as listeners is the person or action that is listening to the event

**Deferred Execution** - The essence of event-driven architecture is that when you publish an event you do *not* wait for a response. The event broker "holds" (persists) the event until all interested consumers accept/receive it, which may be some time much later. Acting on the original event my then cause other events to be emitted, which are similarly persisted.

**Eventual Consistency** - Since you don't know when an event will be consumed and you're not waiting for confirmation, you can't say with certainty that a given database has fully caught up with everything that needs to happen to it, and don't know when that will be the case.

**Orchestration** - the approach in which you introduce a master service dedicated to keeping track of all the other services, taking action if there's an error. Offers a reference when tracing a problem, but also a single point of failure and bottleneck.

## Pros & Cons

### Pros

1. Responsiveness
2. Scalability
3. Agility
4. Leads to decoupled software
5. Encapsulation

### Cons

1. Steep learning curve
2. Complexity
3. Loss of transactionality
4. Lineage (events can be lost or corrupted)

## Event Storming

Domain Event - *orange* - something that is happening in the system that is relevant to the business

Policy - *purple* - represents a process occurred by an event that always starts with the keyword whenever. (i.e. - whenever a new account is created we send a confirmation email)

External System - *pink* - are used to define any interaction that is an external provider (i.e. - PayPal)

Command - *blue* - actions initiated by a user or a system (i.e - scheduled jobs). Commands usually reside at the beginning of the event flow to start the event chain

## Legacy solutions

**Publish / Subscribe** - Messages are published to a message broker (RabbitMQ, MSMQ) and the message will be distributed to all consumers. Topics retain messages only as long as it takes to distribute them to current subscribers. The subscriber must continue to be active for it to consume messages.

**Queue** - Messages are published to a queue and the consumer will read from it. Limitation is you can only have one consumer per queue.

Limitations:

1. Message size: There will always be a limit on the size of the message because larger messages may end up slowing the broker down or even causing it to crash.

2. Zero Fault-Tolerance: Once the consumer reads and processes the message but the back-end database is down or bug in the consumer code

[Alternatives](https://google.com)

## Why Kafka?

1. Scalable

2. Fault Tolerant

3. Open Source

4. High throughput of messages - does not support serialization / deserialization

5. Distributed Streaming Platform

    * Can be used as a messaging system - publish / subscribe pattern
    * Distributed storage - ability to store data in a fault tolerant, durable way
    * Data processing - ability to process events as they occur

## Kafka Components / Definitions

1. Broker - software process (exe, daemon, server) that stores messages and categorizes messages as topics
    * Cluster - is multiple brokers in a single environment for message duplication in case of failures

2. Zookeeper - centralized service which all brokers are connected to. As brokers do not know about one another. Responsibilities include
    * Load distribution
    * Understanding broker availability & failures to prevent sending data to that broker
    * Configuration to help the brokers to stay in sync

**Record** - messages transferred to and from the kafka cluster

* Key - any type
* Value - any type
* Timestamp

**Topics** - a category of messages which are stored in a broker, but the list of all topics are stored in the zookeeper

  1. Delete topics - delete data based on some criteria. (i.e. - size, time)

  2. Compaction Topics - paradigm to upsert (insert/update values) kafka messages based on the key of the message.

**Partitions** - are the way that kafka provides redundancy and scalability. The Kafka topic will further be divided into multiple partitions. The actual messages or the data will store in the Kafka partition. It is directly proportional to the parallelism. If we have increased the number of partition then we can run the multiple parallel jobs on the same Kafka topic. If we increase the amount of partitions over the number of brokers multiple partitions will be on a single broker. Limitations of a single partition on a single broker then the topic will be constrained by the broker's IO throughput. Each partition will store *different* messages.

**Message Offset** - A placeholder that defines what the last read message (position) was and corresponds to the message identifier

**Lag** - When the message offset does not equal to the latest message identifier

## Confluent Kafka vs Apache Kafka

[Stack Overflow: What is the difference?](https://stackoverflow.com/a/39709900)

Confluent (Kafka) offers a holistic set of enterprise-grade capabilities that come ready out of the box to accelerate your time to value and reduce your TCO for data in motion. Rather than needing to spend costly development cycles building and maintaining foundational tooling for Kafka internally, you can immediately leverage features built by the world’s foremost Kafka experts to deploy to production quickly and confidently.

Confluent Kafka extends Apache Kafka with a few additional benefits

1. Serverless - automated, fully managed Kafka clusters with zero ops management

2. Elastic scaling - scale up and down from 0 to GBps without over-provisioning infrastructure

3. Infinite Storage / Tiered Storage - Cost-effectively retain data at any scale without growing compute resources

4. High Availability - guaranteed 99.99% uptime SLA wi built-in failover and multi-AZ replication

5. No ZooKeeper management - metadata management completely abstracted away from developers

6. No-touch patching and upgrades - fully optimized infrastructure with zero-downtime patching and upgrades

7. Native Non-Java Clients - C/C++, Python, Go, C#/.NET

## Kafka Cluster Documentation

### *Apache Kafka*

[Microsoft Docs: How to create in Azure Portal](https://docs.microsoft.com/en-us/azure/hdinsight/kafka/apache-kafka-get-started)

[Docker Compose Example](https://code.parts/2020/06/21/kafka-docker-compose-yml/)

### *Confluent Kafka*

[Docker Setup](https://developer.confluent.io/quickstart/kafka-docker/)

[.NET Client Getting Started](https://developer.confluent.io/get-started/dotnet/)

[.NET Example with Wikipedia](https://www.confluent.io/blog/build-cross-platform-kafka-applications-using-c-and-dotnet-5/)
