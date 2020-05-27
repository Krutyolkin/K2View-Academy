# Query Builder Overview

The Query Builder is an embedded visual query building component that allows you to build complex SQL queries on a selected [DB Interface](/articles/05_DB_interfaces/01_interfaces_overview.md) using an intuitive interface. 
The Query Builder window has two tabs: 
* Query, where you can build and run an SQL query on selected DB Tables, Views or Synonyms.
* Results, which displays the results of the executed SQL Query. 

Note: 
The [DB Interface](/articles/05_DB_interfaces/04_creating_a_new_database_interface.md) has a Schema Filter setting which enables filtering the DB Schema’s list that is used by the Query Builder and the [DB Queries](/articles/07_table_population/01_table_population_overview.md) in the DB Interface.

### Opening the Query Builder Window
 The Query Builder window can be opened in several ways. Select one of the following options: 
1.	**Fabric Studio Toolbars Tab** > <img src="https://github.com/k2view-academy/K2View-Academy/blob/master/articles/11_query_builder/images/12_1_1%20icon.png"> **Query Builder**.
2.	**Project Tree**, right click **DB Interface** > **Show Query Builder.**
3.	**Fabric Studio Java Editor**, right click the **Editor** pane > **Open Query Builder** > **Schema**.

![image]

4.	[**Table Population**](/articles/07_table_population/01_table_population_overview.md) or [**Parser Maps**], if the Source Object is a DB Query, double click the **Source Object** or click **Edit QB Query** in the [**Source Object Properties tab**](/articles/07_table_population/04_table_population_properties_tab.md).
5.	[**Logical Unit Schema window**](/articles/03_logical_units/03_LU_schema_window.md), right click and select either **New Table from SQL Based DB Query** or **New Table From SQL Based Root Function** to [create a new LU Table] based on the [SQL query]. Both options open the Query Builder window to build the SQL query.  The LU Table and its population are automatically generated based on the SQL query defined in the Query Builder.
6.	[**Translation object**], the data in a **Translation** field can be validated using the Query Builder if the **Field Type = SQL**. Click **SQL** next to the field to open the Query Builder.
7.	[**Instance Group**], right click and select **Open Query Builder** > **Schema**.
8.	[**Graphit window**] click ![image] **Query Builder** in the **SQL** or **SQL non-prepared** node type.

9.	[**Broadway**], click the **QB button** in the **DbCommand actor** to open the **Query Builder**.   
