# Change Data Capture Overview

Fabric's Change Data Capture (CDC) solution notifies external systems about data changes published via Kafka and also offers cross-instance Search capabilities through its built-in integration with Elasticsearch.

The following HL flow describes the CDC flow and the population of CDC data in consumers:

![CDC HL Flow](images/cdc_hl_flow.png)


- Each CDC message is sent to Kafka. A specific [CDC message](02_cdc_messages.md) is generated for each type of change in the CDC column.

- The Fabric CDC_TRANSACTION_PUBLISHER job publishes CDC changes to Kafka. Each CDC consumer has its own Kafka topic and its own CDC_TRANSACTION_PUBLISHER job. 

- The Fabric CDC_TRANSACTION_CONSUMER job consumes the Search topic from Kafka and updates Elasticsearch. Other consumers must create their own consumer processes to consume Kafka CDC messages. 

Note that publication of CDC changes requires a [predefined implementation](03_cdc_consumers_implementation.md) in the Fabric Studio. When defining an LU in the Fabric Studio, selected LU table columns can be set to publish CDC messages each time they are updated. 

To publish CDC columns to CDC consumers, LUs with CDC indexes must be deployed to Fabric:

- When the LU is deployed to Fabric for the first time, a [CDC Schema](02_cdc_messages.md#cdc-schema) message is published to Kafka to create the CDC indexes in the CDC consumers.
- When the LU is redeployed to Fabric, a [CDC Schema Update](02_cdc_messages.md#cdc-schema-update) message is published to Kafka about the schema's updates in the affected CDC LU tables.



[<img align="right" width="60" height="54" src="/articles/images/Next.png">](02_cdc_messages.md)



