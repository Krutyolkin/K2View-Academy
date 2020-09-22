# CDC Process Architecture

Fabric CDC process aggregates data updates on the [MicroDB](/articles/02_fabric_architecture/01_fabric_architecture_overview.md#211-microdb-) of the LUI and publishes a CDC message to the CDC consumer for committed changes. 

The following diagram describes the CDC process:

![CDC flow](D:/K2View-Academy/articles/18_cdc_and_search/images/cdc_data_flow_diagram.png)

### MicroDB Update

A transaction on the LUI may involve several updates on several LU tales of the LUI. Each update (write) of the MicroDB SQLite file of the LUI, activates SQLite triggers that send the changes to the **CDC Collector**. The CDC Collector publishes a message to Kafka for each insert, update, or delete events on the MicroDB.  Each message has the LUI (iid), the event type, old and new values of each CDC column, PK columns of the LU table, and transaction id.

If the transaction is committed, a **Commit message** will be sent by the **CDC Collector**. 

If the transaction is interrupted, rollbacked,  or failed, a **Rollback message** will by sent by the **CDC Collector**. 

### CDC Collector

The CDC Collector publishes compressed transaction messages to **Kafka**. Kafka has one topic per LU to keep the transaction messages. The partition key is the hashed value of the LUI and the transaction id.

### CDC Publisher

The CDC Publisher consumes the transaction messages from Kafka and creates a MicroDB for each transaction to hold the data changes for a specific instance.

The schema will contain:

· A metadata table – holds the instance id, transaction-id

· A data tables – that holds all the data change messages

o Corresponding table for each “relevant table from the LU schema”

o “Relevant table” - is a table that has at least one column set to be published.

o Table schema, will have the following columns:

§ PK columns – add PK index

§ Message type

§ Serialized message



On “CDC Message”

The row will be added based on the PK.

For example:

\1. Insert message – add it

\2. Upsert - If two inserts for the same PK comes in – two messages will be added instead of 3 (insert,delete,insert). The flow is as follow:

First message – “insert" => add it

Second message “delete” => execute it => will remove the first message => then add it (if there’s already delete message associated with the same key, then this message will not be added)

Third message – “insert” => add it

\3. Update message will be executed & added (by PK of new values).

\4. Delete message will be executed & added



On Rollback

Rollback transaction

Remove Kafka messages for the relevant transaction?



On commit

\1. Commit the transaction.

\2. Publish the messages to a topic designated for ready messages to be consumed by the “Data changes loaders”

\3. When all messages published successfully, acknowledge the messages on the “On going transaction” kafka topic.

(We might need an additional message type - “publish”, for cases where LU transactions is committed but failed right after the MDB save)

Zombie Transaction

This is a thread that will search for a transaction that saved to the storage layer but not notified to the message bus.

It will periodically search for the transaction-id in the transaction-id table on the relevant LUI MDB.

If found, See “On Commit”/OnPublish? in CDC Publisher



