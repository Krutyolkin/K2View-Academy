# Local Installations of Zookeeper, Kafka, and Elasticsearch

### **Installing and Setup of Zookeeper** 

- Download  the  latest Zookeeper from https://zookeeper.apache.org/releases.html . 

  - For example [**apache-zookeeper-3.6.1-bin.tar.gz**](https://eur03.safelinks.protection.outlook.com/?url=http%3A%2F%2Farchive.apache.org%2Fdist%2Fzookeeper%2Fzookeeper-3.6.1%2Fapache-zookeeper-3.6.1-bin.tar.gz&data=02|01|tali.einhorn%40k2view.com|e467cadc9c524812a6df08d86a02b5ce|994f176e677549549f9e0c719b5e9ca0|1|0|637375907175544494&sdata=AgNFm8tEyEyNBi6ROZs67s%2BvqcgF8pgQ4zUtiS6crHk%3D&reserved=0)

- Copy the Zookeeper zip file to your local directory - for example: C:\k2view - and extract the zip file.

- Go to **conf** sub directory under  the Zookeeper directory- for example: C:\k2view\apache-zookeeper-3.6.1-bin\conf directory: 

  - Rename *zoo_sample.cfg* file to *zoo.cfg*
  - Open for edit *zoo.cfg* file . You can use any text editor like Notepad++. Find the edit *dataDir=/tmp/zookeeper*  and replace the  */tmp/zookeeper* by your Zookeeper data directory. For example: 
    - dataDir=C:\k2view\apache-zookeeper-3.6.1-bin\data 

  - It is possible to  change the default Zookeeper port  in *zoo.cfg* file (Default port 2181).

- Open the System Properties of your windows environment and edit the **Environment Variables**:

  - Add **ZOOKEEPER_HOME** Environment variable and populate it by the Zookeeper directory. For example: 
    - ZOOKEEPER_HOME = C:\k2view\apache-zookeeper-3.6.1-bin\
  - Edit the Environment Variable named **Path**   and add ;%ZOOKEEPER_HOME%\bin; 

- Run ZooKeeper by opening a new cmd window from the Zookeeper directory and  type **zkserver**.

  

### **Installing and Setup of Kafka**  

- Download the latest Kafka from http://kafka.apache.org/downloads.html .

  - For example [**kafka_2.12-2.5.0.tgz**](https://eur03.safelinks.protection.outlook.com/?url=http%3A%2F%2Fapache.spd.co.il%2Fkafka%2F2.5.0%2Fkafka_2.12-2.5.0.tgz&data=02|01|tali.einhorn%40k2view.com|e467cadc9c524812a6df08d86a02b5ce|994f176e677549549f9e0c719b5e9ca0|1|0|637375907175564482&sdata=ERKF0Gv2B3pEClzy0rHUb7pETIYlfsFzyNU5Q8arRtk%3D&reserved=0)

- Copy the Kafka zip file to  to your local directory - for example: C:\k2view - and extract the zip file. 
- Go to **config** sub directory under  the Kafka directory- for example: C:\K2View\kafka_2.12-2.5.0\config directory: 
  - Open for edit *server.properties* file. Find and edit the line *log.dirs=/tmp/kafka-logs* and replace the */tmp/kafka-logs* by your Kafka local directory. For example:
    - log.dir= C:\K2View\kafka_2.12-2.5.0\logs
- Start Kafka, open a new cmd window from the Kafka directory and run the following command in the cmd window:
  - .\bin\windows\kafka-server-start.bat .\config\server.properties

  

#### **Creating a Producer and Consumer to Test Server** 

- Open a cmd window from [Kafka directory]\bin\windows directory. For example: C:\K2View\kafka_2.12-2.5.0\bin\windows.
- Run the following command in the cmd window to start a  producer type:

  - kafka-console-producer.bat --broker-list localhost:9092 --topic test

- Open again a new cmd window in the same location and run the following command in the cmd window to start a consumer:

  - kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test

 **Notes:**

- The local Kafka will run on default port 9092 and connect to ZooKeeper’s default port, 2181.
- Update the config.ini and iifConfig.ini Fabric files and replace the Kafka port from 9093 to 9092.

### Installing the Elasticsearch 

Please follow the instructions in the following link:  https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-windows.html .