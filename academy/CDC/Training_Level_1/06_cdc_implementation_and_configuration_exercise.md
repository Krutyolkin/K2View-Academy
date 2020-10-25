# CDC Implementation and Configuration

### ![](/academy/images/Exercise.png) Exercise - Add CDC Fields to Customer LU

In this exercise you will add a new CDC consumer - CDCTraining - to your project, and add CDC fields om Invoice LU table of Customer LU to publish the Invoice's changes to the CDCTraining Kafka topic. 

This exercise includes the following steps:

* Add a new CDC consumer- CDCTraining - to your project.
* Add CDC fields on Invoice LU table. 
* Configure the CDC sections in the config.ini file.
* Deploy the updated Customer LU and check Kafka messages of CDCTraining topic.
* Get an instance to Customer LU and check the Kafka messages of CDCTraining topic. 

Read [CDC Implementation Steps](/articles/18_fabric_cdc/03_cdc_consumers_implementation.md) and [CDC Configuration](/articles/18_fabric_cdc/06_cdc_configuration.md) to learn how to add a CDC implementation and configuration to your working environment.

### Prerequisites

- Download the [Demo Project](/articles/demo_project) and its related SQLite interfaces to your Fabric Studio.
- Install Zookeeper and Kafka server.
- Start Zookeeper and Kafka server.

[Click to get installation instructions](/articles/demo_project/01_local_installation_of_zookeper_kafka_and_ES.md).  

### **Exercise Steps**:

##### Step 1 - CDC Data Publish Configuration

- Open the config.ini file of your local Fabric server. To open the config.ini file of the local Fabric server do the following:

  - Open the Demo Project using the Fabric Studio. Go to the project tree, right click the project name > Open Folder. A popup window is opened with the project's folder.
  - Select FabricHome/config directory under the project's folder and open for edit the config.ini file.

- Edit the [cdc_data_publish] section, uncomment the **BOOTSTRAP_SERVERS** and populate it by the host and port of the Kafka server. Note that if you use a local installation of the Kafka server, you need to edit this parameter as follows: 

  - BOOTSTRAP_SERVERS=localhost:9092

  [Click to read more about the CDC configuration](/articles/18_fabric_cdc/06_cdc_configuration.md). 

  ##### Step 2 - Adding a CDC Consumer to the Fabric Project

- Open the Demo Project using the Fabric Studio. Go to the project tree, right click the project name > Open Folder. A popup window is opened with the project's folder.

- Open the [project name].k2proj file for edit.

- Add a a new CDC consumer named **CDCTraining** to the **DataChangeIndicators** tag. Set the data type of the CDC field in the **option** tag to **CDCdata**.

- Save and close the k2proj file.

- Close and reopen the project in the Fabric Studio to reload the updated k2proj file.

  [Click to read more about adding CDC consumers to the Fabric project](/articles/18_fabric_cdc/03_cdc_consumers_implementation.md#adding-cdc-consumers).

##### Step 3 - Edit Customer LU - Adding CDC Fields to Invoice LU Table

- Open Invoice table of the Customer LU. Verify that a new tab named **CDCTraining** is added to the table's window.

- Define a **CDCTraining** fields (indexes) on the following LU table's columns:

  - INVOICE_ID
  - SUBSCRIBER_ID
  - STATUS
  - DUE_DATE
  - BALANCE

- Save the changes and close the LU table window.

  [Click to read more about adding CDC fields to the LU table](/articles/18_fabric_cdc/03_cdc_consumers_implementation.md#creating-indexes-for-other-cdc-consumers).

##### Step 4 - Deploy the Customer LU 

- Open the Fabric console and check the list of CDC jobs using **jobstatus** command.

- Deploy the Customer LU to the local Fabric server.

- Recheck the list of CDC jobs using **jobstatus** command.

  **Questions**:

  <ul>
  <pre><code>
  1. Which job was added to the jobs list after the deploy of Customer LU?
  2. What are the Type and the UID of the new Job? 
  3. What happens to the jobs list if you add CDC fields to CRM LU and redeploy your changes? 
  </code></pre>
  </ul>

- Open the Kafka server console and run a command to get the list of Kafka topics. If you use a local Kafka installation, do the following:

  - Open a cmd window from the [local Kafka Windows directory]\bin\windows

  - Run the following command: 

    **kafka-topics.bat --zookeeper localhost:2181 -list**

    [Click to get the list of Kafka basic commands](/articles/02_fabric_architecture/08_kafka_basic_commands.md).

    

  **Question:**

  <ul>
  <pre><code>
  1. Which topic was added for the CDCTraining consumer? What is the relation between the CDC Consumer name and the Kafka topic name?
  </code></pre>
  </ul>

[![Previous](/articles/images/Previous.png)](05_cdc_consumer_example_using_broadway_as_cdc_consumer.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](06_cdc_implementation_and_configuration_exercise.md)

