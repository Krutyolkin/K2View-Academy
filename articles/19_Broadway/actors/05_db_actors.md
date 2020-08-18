# DB Commands Actors 

Broadway has a group of built-in **db** Actors that are useful for performing DB commands and actions like creating a new table, loading data or fetching it and executing other DB commands. These Actors are:
- **DbCommand** Actor, performs database commands against a DB interface. It has two extensions: 
  - **DbFetchField** Actor, returns the first field of the first row or null if not present.
  - **DbFetchFirstRow** Actor, returns the first row or an empty row if no result is present.
- **DbCreateTable** Actor, creates a new database table.
- **DbLoad** Actor, loads data into a database using an INSERT, UPDATE or UPSERT command.

Each **db** Actor category requires setting an **interface** input argument that can be either a reference to the Fabric [DB Interface](/articles/05_DB_interfaces/03_DB_interfaces_overview.md) or a JDBC URL. 

The query defined in the Actor can contain either ordered parameters using **?** or named parameters using **${}** notation. The values of named parameters are taken from the Actor's input parameters or from the **params** input argument and only if it is a map. For ordered parameters, the **params** input argument should be an array or a single value (not a map).

### How Can I Load the Data?

Data can be loaded in a Broadway flow using either a **DbCommand** Actor or a **DbLoad** Actor.

To load the data using the **DbCommand** Actor, write the SQL INSERT statement in the **sql** input argument. The values to be populated in the table can be taken from the input arguments using the named parameters. For example:

​	`INSERT INTO DATA (TEXT) VALUES (${text} )`

Where **${text}** is replaced with the value of the **text** input argument in the prepared statement.

Another way to load the data in a Broadway flow is by using the **DbLoad** Actor. To do so, populate the Actor's input arguments:

* **command**, select INSERT, UPDATE or UPSERT from the dropdown list.
* **schema**, **table**, either type it in or click the **DB** button to select it from the DB Table Selection popup. 
* **fields, keys**, if a table has been selected, the fields and keys are automatically populated from the DB schema. If not, type in the field names.

### DB Commands Examples

The **db-commands.flow** example shows how the **DbCommand** Actor can be used to perform various DB actions, including:

* Creating a table using the SQL provided in the **sql** input argument.
* Inserting the data into the table using the **sql** input argument with named parameters marked as **${text}**. The parameter's values are populated from the **params** input argument.
* Selecting the data from the table using the **sql** input argument with an ordered parameter marked as **?**. 

Click **Actions** > **Examples** in the [Main menu](../18_broadway_flow_window.md#main-menu) to open the **db-commands.flow** example. 

![image](../images/99_actors_05_1.PNG)





[![Previous](/articles/images/Previous.png)](04_queue_actors.md)
