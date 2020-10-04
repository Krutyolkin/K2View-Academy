# Using Broadway as CDC Consumer

Broadway has a [queue built-in Actors](/articles/19_Broadway/actors/04_queue_actors.md) that can manage Pub or Sub asynchronous message handling and can subscribe to Apache Kafka messages. 

Since Fabric publishes the [CDC messages](/articles/18_fabric_cdc/02_cdc_messages.md) to Kafka, it is possible to define a dedicated CDC consumer for Broadway and subscribe the CDC messages by the [Broadway flow](/articles/19_Broadway/02a_broadway_flow_overview.md).

## Adding a CDC Consumer for Broadway

You can customize Fabric Studio and add an additional CDC consumer for Broadway flow as follows:

1.  Go to the project tree, right-click the project name and select the **Open Folder** option.

2.  Open the [project name].k2proj file for editing.

3.  Edit the **DataChangeIndicators** tag by adding the **DataChange** tag. By default, the **DataChangeIndicators** contains the **Search** consumer.  Set the consumer name in the **name** and set the **Option ** in the **Options** tag to **data**.  See the example below:

```
<DataChangeIndicators>
    <DataChange name="Search" enabled="true">
      <Options>
        <option>keyword</option>
        <option>data</option>
        <option>date</option>
      </Options>
    </DataChange>
	<DataChange name="CDCBroadway" enabled="true">
      <Options>
        <option>data</option>
      </Options>
    </DataChange>
  </DataChangeIndicators>
```

## Creating CDC Indexes for Broadway

Select the LU tables' columns that needs to be published to the CDC topic of Broadway. 



[Click for more information about CDC Implementation Steps](04_cdc_consumers_implementation.md). 



## Creating Kafka Interface

Create a new Interface. Set the **Interface Type** to **Kafka**.  Populate **Bootstrap Servers**  by the host and port of the Kafka server. For example: localhost:9092. Note that you can set several servers, delimited by a comma.

## Creating a Broadway Flow to Consume the CDC Messages

Define a Broadway flow. Add the **Subscribe** built-in actor. Set the input parameters of the Subscribe actor as follows:

-Set the interface by the Kafka interface, added to the Fabric project.

- Set the topic by the CDC consumer name (CDCBroadway in the example above), since the topic name of each CDC consumer is identical to the CDC consumer name.

  [Click for examples of Pub/Sub Broadway flows](/articles/19_Broadway/actors/04_queue_actors.md#pub--sub-examples).



