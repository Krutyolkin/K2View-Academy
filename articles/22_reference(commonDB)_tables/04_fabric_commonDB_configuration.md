# CommonDB - Fabric Configuration 

## CommonDB Configuration Settings

All configuration parameters are located in the *common_area_config* section of the config.ini file located in the *Fabric Home directory* of each Fabric Node.


### Update Size
The allowed size for an update must be configured using the UPDATE_SIZE and SNAPSHOT_SIZE parameters in the *common_area_memory_queues_config* section

```
# Memory queue size for update - e.g. 10 MB
UPDATE_SIZE=10000
```

```
# Memory queue size for snapshot
#SNAPSHOT_SIZE=100
```



### Update Distribution
Fabric allows user whether to use Fabric or server's RAM to distribute a message - for single-node configuration only

```
# Messages distribution mode - MEMORY / KAFKA
MESSAGES_BROKER_TYPE=MEMORY
```

### Message Access Retry

```
# Max retry for a poisoned message
PROCESS_UPDATE_MESSAGE_RETRIES_COUNT=3
```

### Snapshot Idle Time

```
# Max idle time when consuming snapshot messages, if processing hangs longer the consumer will return IDLE_TIMOUT
# 16.6 hrs
CONSUMER_IDLE_TIME=60000
```

### Max retry for common area operation
```OPERATION_RETRIES_COUNT=3```

### Affinity 

Using the [affinity](/articles/20_jobs_and_batch_services/10_jobs_and_batches_affinity.md) parameter, The sync table process can be allocated to a specific node (default: no affinity)

```
SYNC_JOBS_AFFINITY=10.23.10.11
```

### Drop Topics

When set to true, all topics for tables being dropped will be removed automatically:

```
DELETE_TOPICS_ON_DROP=true
```

### Bulk Size

If 2500 insert commands are required, each bulk of 1000 commands is sent to Kafka, while the update content is kept in Cassandra. 
These 2500 inserts are divided into 3 transactions, the first 2 containing 1000 rows to insert, and the third one 500.

```
TRANSACTION_BULK_SIZE= 1000
```


### MAXIMUM TRANSACTIONS in BULK

```
MAX_TRANSACTIONS_COMMIT=100
```


### Memory queue size for snapshot
```
# defines maximum snapshot size - in the case below set to 100 MB
#SNAPSHOT_SIZE=100
```

### Snapshot table TTL

```
# in seconds - Default is one week
# below has been set to one day
COMMONS_TABLE_TTL=86400 
```

## CommonDB Kafka-Related Configuration Settings

All configuration parameters are located in the *common_area_kafka_producer* and *common_area_kafka_consumer* sections of the Config.ini file located in the *Fabric Home directory* of each Fabric Node:

### Kafka Producer

```
#ACKNOWLEDGMENT=all
#RETRIES=0
#BATCH_SIZE=16384
#BOOTSTRAP_SERVERS=localhost:9093
#LINGER_MS=1
#MAX_BLOCK_MS=60000
#REPLICATION=3
#BUFFER_MEMORY=33554432
#KEY_SERIALIZER=org.apache.kafka.common.serialization.LongSerializer
#VALUE_SERIALIZER=org.apache.kafka.common.serialization.ByteArraySerializer
```

### Kafka Consumer

```
#AUTO_OFFSET_RESET=earliest
#BOOTSTRAP_SERVERS=localhost:9093
#ENABLE_AUTO_COMMIT=false
#MAX_POLL_RECORDS=500
#MAX_POLL_INTERVAL_MS=5000
#SESSION_TIMEOUT_MS=30000
#KEY_DESERIALIZER=org.apache.kafka.common.serialization.LongDeserializer
#VALUE_DESERIALIZER=org.apache.kafka.common.serialization.ByteArrayDeserializer
#DEFAULT_DURATION_TIME=100
```

### Kafka Security

Fabric supports the following Kafka security encryption, authentication and authorization configuration parameters: 

```
#SSL_ENABLED=false
#SECURITY_PROTOCOL=SSL
#TRUSTSTORE_LOCATION=kafka.client.truststore.jks
#TRUSTSTORE_PASSWORD=
#KEYSTORE_LOCATION=kafka.client.keystore.jks
#KEYSTORE_PASSWORD=
#KEY_PASSWORD=
#ENDPOINT_IDENTIFICATION_ALGORITHM=
#SSL_CIPHER_SUITES=
#SSL_ENABLED_PROTOCOLS=
#SSL_TRUSTSTORE_TYPE=
```




## Cassandra Snapshots Settings

### SNAPSHOTS NAME
```
COMMONS_SNAP_TABLE=snapshots
```

### Entry TTL 
```
Snapshot table TTL in seconds - Default is one week
#COMMONS_TABLE_TTL=8640
```

### TIMEOUT

Maximum idle time when consuming snapshot messages
```
CASSANDRA_WAIT_MESSAGE_TIMEOUT=60000
```


[<img align="left" width="60" height="54" src="/articles/images/Previous.png">](/articles/22_reference%28commonDB%29_tables/03_fabric_commonDB_flow.md)

[<img align="right" width="60" height="54" src="/articles/images/Next.png">](/articles/22_reference%28commonDB%29_tables/05_fabric_commonDB_runtime.md)
