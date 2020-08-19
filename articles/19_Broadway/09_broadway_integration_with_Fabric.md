# Broadway Integration with Fabric Studio

Broadway is a Fabric module that can run either standalone in a Web browser or embedded in the Fabric Studio. 

The Fabric Studio includes several integration points that are used by Broadway Actors to simplify the creation of Broadway flows.

### Interfaces List

Several Broadway [Actors](03_broadway_actor.md) include an **interface** as one of their input arguments. When setting the Actor's interface from the dropdown list, the list of values is retrieved from the Project Interfaces list. Only Active interfaces are displayed. The values are filtered by the Interface Type where only interfaces relevant to the Actor type are shown.

For example:

* [DB Commands Actors](actors/05_db_actors.md), the interface only displays the Fabric [DB Interfaces](/articles/05_DB_interfaces/03_DB_interfaces_overview.md).
* [Pub / Sub Actors](actors/04_queue_actors.md), the list of interfaces is limited to Kafka or JMS interface types. 
* [**FileRead** /**FileWrite** Actors](actors/02_stream_actors.md), the list of interfaces includes SFTP and local file system interface types. 

Note that the list of values in the Actor's **interface** dropdown list also includes the custom values applicable for the Actor type that are not retrieved from Project interfaces. For example, **custom url**. 

### Table Selection and Query Builder

The input arguments of [DB Commands Actors](actors/05_db_actors.md) include integration to Fabric windows which simplifies the creation and validation of queries. 

To populate the SQL statement of a **DbCommand** Actor, do the following:

1. Set the **interface** input argument and then click **QB** in the **sql** input argument field. The [Query Builder window](/articles/11_query_builder/02_query_builder_window.md) opens and is filtered by the selected DB connection.
2. Click the required table and fields or write the query manually. 
3. Click **Execute Query** to validate it and then click **OK**. The SQL statement is populated in the **sql** input argument of the **DbCommand** Actor.

To set the table and the fields of a **DbLoad** Actor, do the following:

1. Set the **interface** input argument and click **DB** in the **table** input argument field. The Table Selection popup window  opens and is filtered by the selected DB connection.
2. Click the required **table** and then **OK** to populate the table name and the columns in the **table** and the **fields** input arguments of the **DbLoad** Actor.

<table>
<tbody>
<tr>
<td valign="center" ><img src="images/99_07_SQL.PNG" alt="SQL" /></td>
<td valign="center" ><img src="images/99_07_DB.PNG" alt="DB" /></td>
</td>
</tr>
</tbody>
</table>

### LuFunction and Graphit Actors

The **LuFunction** and **Graphit** Actors fully utilize Fabric integration with Broadway to enable the reuse of the Fabric logic within Broadway flows. 

To do so, set the [Logical Unit](/articles/03_logical_units/01_LU_overview.md) in the **luType** input argument and then select a [Project function](/articles/07_table_population/08_project_functions.md) or a [Graphit](/articles/15_web_services_and_graphit/17_Graphit/01_graphit_overview.md) resource. 

Note that the **luType** input argument includes the list of all Logical Units in the Project including the [Web Services](/articles/15_web_services_and_graphit/01_web_services_overview.md) and the References. It is recommended to use the same LU as the one where the Broadway flow is created.

#### LuFunction Actor

The **LuFunction** Actor is used when a Fabric [Project function](/articles/07_table_population/08_project_functions.md) must be invoked from a Broadway flow and is also a way to write business logic in Java rather than in JavaScript in Broadway. 

After the **luType** input argument is set, the list of values in the **functionName** dropdown list are filtered by the LU name and display the functions of the selected LU and Shared Object. The Actor's input and output arguments are updated with the inputs and output of the selected function.

#### Graphit Actor

The **Graphit** Actor executes Graphit logic for data serialization. Parameters to the Graphit execution are picked up from input arguments or from the **params** input argument.

After the **luType** input argument is set, the list of values in the **graphit** dropdown list is filtered by the LU name. The Actor's input and output arguments are updated with the inputs and output of the selected Graphit resource.

### Fabric Commands Actors

The **fabric** category of [built-in Actors](04_built_in_actor_types.md) executes Fabric commands.

* **FabricGet** Actor, executes the GET command on the current Fabric session.
* **FabricSet** Actor, sets a value on the Fabric session.
* **FabricSetRead** Actor, reads a key from the Fabric session.

The **sql** input argument of these Actors displays the command to be executed on the [Fabric Interface](/articles/05_DB_interfaces/05_adding_a_fabric_and_remote_fabric_interface_type.md). This argument is read-only and contains named params using ${} notation. 

For example, **FabricGet** Actor displays the command:

~~~
GET ${luType}.${iid}
~~~

Where **${luType}** and **${iid}** are replaced by the values of the input arguments in the prepared statement. 

Select the [Logical Unit](/articles/03_logical_units/01_LU_overview.md) in the **luType** input argument and type the [Instance ID](/articles/01_fabric_overview/02_fabric_glossary.md#instance-id) in the **iid** input argument.

![image](images/99_07_FABRIC.PNG)

### Broadway as a Population

A Broadway flow can be used as a [Table population](/articles/07_table_population/01_table_population_overview.md) to replace  complex Java code in the population logic from [Stages](19_broadway_flow_stages.md) and [Actors](03_broadway_actor.md) in the flow. 

To create the population based on the Broadway flow, right click the table name in the **Project Tree** and select **New Table Population Based Broadway Flow**. The population template is created and can be modify as needed.

![image](images/99_07_POPULATION.PNG)



### Interface Listener For Broadway Flows

The Interface Listener functionality, an enhancement of the Fabric Jobs functionality, can be used to define a SFTP or a local file system listener for a Broadway flow. The Listener Job is attached to a Broadway flow and triggers the flow when a file with the predefined path arrives to the predefined interface.

![image](images/99_07_JOBS.PNG)




[![Previous](/articles/images/Previous.png)](06_export_actor.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](17_tutorial_and_flow_examples.md)
