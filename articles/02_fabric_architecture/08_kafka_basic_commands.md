# Kafka Basic Commands

### Useful Kafka Commands - Linux Server

- Start Zookeeper

  ```
  $K2_HOME/kafka/bin/zookeeper-server-start -daemon $K2_HOME/kafka/zookeeper.properties
  ```

  

- Start Kafka

  ```
  $K2_HOME/kafka/bin/kafka-server-start -daemon $K2_HOME/kafka/server.properties
  ```

  

- Check Kafka Brokers are Up
  ```
  $K2_HOME/kafka/bin/zookeeper-shell <ZOOKEEPER IP>:2181 <<< "ls /brokers/ids"
  ```

  

- Stop Kafka
  ```
  $K2_HOME/kafka/bin/kafka-server-stop
  $K2_HOME/kafka/bin/zookeeper-server-stop 
  ```

  

- Get the List of Kafka Topics
  ```
  $K2_HOME/kafka/bin/kafka-topics --zookeeper <ZOOKEEPER IP>:2181 –list
  ```

  

- Create topic 
  ```
  $K2_HOME/kafka/bin/kafka-topics --create --replication-factor <n> --partitions <k> --topic <topicName> --zookeeper <ZOOKEEPER IP>:2181
  ```

  

- Start Kafka Consumer on a Specific Topic 

  ```
  $K2_HOME/kafka/bin/kafka-console-consumer --topic <topicName> --bootstrap-server <server ip>:9093
  ```



- Delete Kafka Topic   

  ```
  $K2_HOME/kafka/bin/kafka-topics --delete --topic <topicName> --zookeeper <zookeeper ip>:2181
  ```



- Start Kafka Consumer on Multiple Topics Based on RegEx  

  ```
  $K2_HOME/kafka/bin/kafka-console-consumer  --whitelist ‘^(IDFINDER).*’ --bootstrap-server <server ip>:9093 
  ```



- Start Kafka Consumer on a Specific Topic from the Beginning  

  ```
  $K2_HOME/kafka/bin/kafka-console-consumer --topic <topicName> --bootstrap-server <server ip>:9093 --from-beginning
  ```

 

- Check for a Lag on Messages Consumption  

```
 $K2_HOME/kafka/bin/kafka-consumer-groups --group <groupID> -- describe --bootstrap-server <server ip>:9093
```



- Get a List of Consumer Groups 

  ```
   $K2_HOME/kafka/bin/kafka-consumer-groups --bootstrap-server <server ip>:9093 –list
  ```



- Count the Messages Published to Kafka So Far  

  ```
   $K2_HOME/kafka/bin/kafka-run-class kafka.tools.GetOffsetShell --broker-list <server ip>:9098 --topic <TOPIC> --time -1 --offsets
  ```



Note that if you run Kafka on a windows server, you need make the following adjustments on he commands above: 

- Run the *.bat scripts from the cmd window. 

  **Examples:**

  - To start Kafka on windows server, run the following steps:

    - Open a new cmd window in the same location as [Kafka local directory]

    -  Run the following command on the cmd window:

      .\bin\windows\kafka-server-start.bat .\config\server.properties 

  - To start a Kafka consumer on a "test" topic, do the following:

    - Open a new cmd window in the same location as [Kafka local directory]\bin\windows

    - Run the following command on the cmd window:

      kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test

- The local Kafka installation runs on a default port 9092. Edit the commands above and replace the port from 9093 to 9092.

- [Click for more information about local installation of Kafka and Zookeeper](/articles/demo_project/01_local_installation_of_zookeper_kafka_and_ES.md).

  

[![Previous](/articles/images/Previous.png)](07_cassandra_basic_commands.md)

