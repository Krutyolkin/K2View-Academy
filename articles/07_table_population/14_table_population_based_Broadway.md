# Table Populations Based on Broadway Flows

A [Table Population](/articles/07_table_population/01_table_population_overview.md) defines and executes mapping and transformation rules from a data source to a target. A table population can be created based on a source object or based on a Broadway flow. 

A [Broadway flow](/articles/19_Broadway/02a_broadway_flow_overview.md) is a core Broadway object that represents a business process and is built from several [Stages](/articles/19_Broadway/19_broadway_flow_stages.md) where each Stage includes one or more [Actor](/articles/19_Broadway/03_broadway_actor.md).

The advantages of using a Broadway flow for table population rather than a source object based population are:

* Streamlining logic and all related validations into one business process whereby improving the project's maintainability.
* Populating more than one table in a single population flow.
* Replacing the source DB with other action such as an HTTP call.

[Click for more information about Broadway](/articles/19_Broadway/01_broadway_overview.md).

### Flow Population Template

A Broadway population flow template includes predefined Stages and designated Actors and can be modified by adding more Actors when needed. 

The following Broadway flow is a default template created to populate the CASES table which is connected to the parent's ACTIVITY table in a Customer LU.

![image](images/07_14_01.PNG)



![image](images/07_14_03.PNG)

The default population flow template includes the following Stages and Actors:

* **Input** Stage, defines the population's input arguments using a designated **PopulationArgs** Actor. 

  * Input arguments are either added automatically based on the selected table's fields or must be added manually. 
  * The **iid** output argument indicates the instance ID of the execution. The **parent_rows** output argument is an array of objects that iterate over parent rows. For example, the **iid** is a customer ID and the **parent_rows** includes the list of activity IDs of this customer.

* **Source** Stage, defines a query that retrieves source data using the **SourceDbQuery** Actor. 

* The **SourceDbQuery** Actor inherits from the [**DbCommand** Actor](/articles/19_Broadway/actors/05_db_actors.md) and extends it with additional **parent_rows** and **size** input arguments whereby improving the Actor's performance. 

  * The interface for the query's execution is selected from the list of Fabric [DB interfaces](/articles/05_DB_interfaces/03_DB_interfaces_overview.md). 

  * A query is either populated automatically in the **sql** input argument or must be added manually. A query can be validated in the [Query Builder window](/articles/11_query_builder/02_query_builder_window.md) by clicking **QB** in the **sql** input argument field. 

  * The **size** value is used to group the rows from **parent_rows** where each group is used to generate the WHERE clause for the provided SQL statement. The **size** is important for the Actor's performance since it enables generating less calls to the source DB.

  * The WHERE clause is generated automatically in the same way as for regular populations and is not visible in the Actor's UI. 

    For example, when the **sql** input argument includes:

    ~~~sql
    SELECT * FROM CASES
    ~~~

    The SQL statement that will actually be executed is:

    ~~~sql
    SELECT * FROM CASES WHERE ACTIVITY_ID IN (...)
    ~~~

  * Additional parameters can be added to the WHERE clause if needed. For example, to filter cases by their status.

  * The **SourceDbQuery** Actor supports [non-prepared statement parameters](/articles/19_Broadway/actors/05_db_actors.md#example-of-non-prepared-statement). For example, to dynamically transfer a table or column name to a query.

* **Stage 1**, an empty Stage added to the template to indicate that additional activities can be performed on the data prior to loading it to the target DB, like it si done by a [Root function](/articles/07_table_population/02_source_object_types.md) in a regular population object.  

* **LU Table** Stage, defines the target LU table using the **DbLoad** Actor. 

  * The target **interface**, **schema**, **table** and INSERT, UPDATE or UPSERT **commands** are set using the Actor's input arguments. 
  * The [link type](/articles/19_Broadway/07_broadway_flow_linking_actors.md#link-object-properties) from the Query to the load is set as **Iterate** to enable looping over the query results.
  * Note that by default, **schema** and **table** input arguments are defined as an [**External** population type](/articles/19_Broadway/03_broadway_actor_window.md#actors-inputs-and-outputs) to enable populating these parameters dynamically. When required, a **Const** or **Link** population type can be defined. 

* **Post Load** Stage, an empty Stage added to the template to indicate that additional activities can be performed after the data has been loaded to the target DB, like it is done by an [Enrichment function](/articles/10_enrichment_function/01_enrichment_function_overview.md). If not needed, this Stage can be deleted or left empty.

### How Do I Create a Population Based on a Broadway Flow?

The starting points for creating a population based on a Broadway flow are:

* [Auto Discovery Wizard](/articles/03_logical_units/06_auto_discovery_wizard.md), check the **Table population based Broadway flow** checkbox in step 2 of the Wizard.
* [LU Schema window](/articles/03_logical_units/03_LU_schema_window.md#logical-unit-lu-schema), either:
  * Right click and select **New table from SQL based Broadway flow**.
  * Drag a DB table and select **Create table based Broadway flow**.
* Project Tree, right click a table object and select **Create Table Population based Broadway Flow**.
* Reference, right click and select **Create References from DB tables**.

The population is created as a template with predefined Stages and designated Actors. When creating a flow from the Auto Discovery Wizard or from the LU Schema, the input fields, interface and SQL statement are added automatically based on the selected table's fields. Complete the missing information and if needed, update the flow and then connect the table population to the LU hierarchy via the LU Schema window.

[Click for more information about building an LU hierarchy and linking table populations](/articles/03_logical_units/12_LU_hierarchy_and_linking_table_population.md).

Note that for the population to be effective on the server side, deploy the population's Logical Unit.

[Click for more information about deployment from the Fabric Studio](/articles/16_deploy_fabric/02_deploy_from_Fabric_Studio.md).

### Example of Creating a Population Based Broadway Flow

1. In the **DB Objects tab** of the **LU Schema**, drag the required table into the main area and click **Create Table based Broadway Flow**.

2. Enter the **population name** and click **OK** to open a Broadway flow window. The flow's template is created and includes the basic steps for retrieving  source data and loading it into the target. 

   ![image](images/07_14_02.PNG)



3. Connect the required input arguments of the **PopulationArgs** Actor to the relevant port of the parent table in the LU Schema. 


4. (Optional) Add the WHERE clause to the **sql** input argument of the **Query** Actor.

### How Do I Transfer Additional Parameters to a Where Clause?

The WHERE clause is generated automatically and includes filters by the parent rows. However, if additional filters are needed, they can be added manually to the **sql** input parameter of  the **SourceDbQuery** Actor.

Generally, parameters can be transferred to the WHERE clause of the **SourceDbQuery** Actor in the same way as they are transferred to the [**DbCommand** Actor](05_db_actors.md).

For example, to filter cases according to a status, the SQL statement is:

~~~sql
Select * From CASES
Where CASES.STATUS = ${VALUE}
~~~

When the above query is written in the **sql** input parameter, a new input argument **VALUE** is added to the **SourceDbQuery** Actor and the parameter should be transferred to it as shown below: 

![image](images/07_14_04.PNG)



[Click to display an example of a population flow in the Demo project.](/articles/demo_project)

[![Previous](/articles/images/Previous.png)](13_LU_table_population_execution_order.md)
