# Alternatives to Apache Kafka

## Database Replication & Log Shipping

Limitations:

1. RDBMS (Relational Database Management System) to RDBMS only

2. Database-specific

3. Tight coupling between the source and the target (schema change)

4. Performance challenges (log shipping)

5. Cumbersome (subscriptions)

## Extract, Transform, and Load (ETL)

Limitations:

1. Typically proprietary and costly

2. Lots of custom development

3. Scalability challenges

4. Performance challenges (multiple instances are typically required)

## Messaging (non-Kafka)

Moves simple messages between systems and datastores

Limitations:

1. Limited scalability (broker)

2. Smaller messages (message and data packet size)

3. Requires rapid consumption

4. No fault-tolerance (application) - if an application pops the message off the queue and processes it incorrectly it is extremely difficult to get it to reprocess
