# CDC Consumer Example: Using Broadway as CDC Consumer

[Broadway](/articles/19_Broadway/01_broadway_overview.md) has a queue of [built-in Actors](https://github.com/k2view-academy/K2View-Academy/blob/Academy_6.2_CDC_Tali/articles/19_Broadway/actors/04_queue_actors.md) that manage the handling of Pub or Sub asynchronous messages and can subscribe to Apache Kafka messages.

Since Fabric publishes the [CDC messages](https://github.com/k2view-academy/K2View-Academy/blob/Academy_6.2_CDC_Tali/articles/18_fabric_cdc/02_cdc_messages.md) to Kafka, a dedicated CDC consumer can be defined in Broadway to subscribe CDC messages in a [Broadway flow](https://github.com/k2view-academy/K2View-Academy/blob/Academy_6.2_CDC_Tali/articles/19_Broadway/02a_broadway_flow_overview.md).