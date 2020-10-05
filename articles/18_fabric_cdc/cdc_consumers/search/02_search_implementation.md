# Search Implementation Steps

## Creating Search Engine Interface 

- The Search Engine interface is required to enable Fabric connection to the Elasticsearch engine when running the search commands.
- The Search Engine interface is populated  with the connection details of the Elasticsearch engine for your search.
- To  create a new Search Engine interface:

1. Go to **Project Tree** > **Shared Objects**, right click **Interfaces** and select **New Interface**.
2. Select **Search Engine** from the drop-down menu.
3. Populate the connection settings.

### Search Engine Interface Settings

<table>
<tbody>
<tr>
<td valign="top" width="200pxl">
<p><strong>Parameter </strong></p>
</td>
<td valign="top" width="700pxl">
<p><strong>Description </strong></p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p>Host(s)</p>
</td>
<td valign="top" width="700pxl">
<p>Comma-delimited list of hosts and ports of the Elasticsearch servers.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p>User</p>
</td>
<td valign="top" width="700pxl">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p>Password</p>
</td>
<td valign="top" width="700pxl">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p>SSL</p>
</td>
<td valign="top" width="700pxl">
<p>By default- set to False.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p>KeyStore Path</p>
</td>
<td valign="top" width="700pxl">
<p>Key Store path in case SSL is set to True.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p>KeyStore Password</p>
</td>
<td valign="top" width="700pxl">
<p>Key Store Password in case SSL is set to True.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p>KeyStore Type</p>
</td>
<td valign="top" width="700pxl">
<p>Key Store Type in case SSL is set to True.</p>
</td>
</tr>
</tbody>
</table>

## Fabric Studio- Defining Search Fields

It is required to define Search fields on the selected LU tables' columns to enable a cross-instance search based on these columns. For example, to enable a search of all customers called "John Doe", it is required to define FIRST_NAME and LAST_NAME columns of the customer schema as Search Indexes.

To create a Search index on a column, do either: 

- Right click the selected column > **Create Search Index From Selected Columns** > select the index type.
- Open the **Search** tab populate the **Column Name** and **Type**. 

Note that Fabric Studio does not enable defining more than 63 columns of the same LU table as CDC fields, assuming that all columns are positioned according to 1 to 63 in the LU table.

### Search Fields Types

The default built-in types of the Search fields are:

- **keyword**, enables a search by this column. Behind the scenes, Fabric creates two Search Indexes for the keyword field: keyword index and additional index based on the type of the field. The keyword index enables performing a search by exact match (case sensitive) to the searched value. 
- **date**, enables a search on a date column.  Date fields must be populated by a date format, identified by the Elastic Search. See the list of the date formats, supported by the Elastic Search:
  * [https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#built-in-date-formats](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#built-in-date-formats)
- **data**, can be returned by the search, but you cannot initiate a search by this column.
- [search templates](04_search_templates.md)- Fabric supports adding templates for search fields to support special cases.

**Example:** 

- Set the Search Indexes of Customer LU as follows:
  - First Name and Last Name are keyword fields.
  - Customer ID is a data field.
- You can run a search to get the list of all customer IDs called “John Doe”. 



[![Previous](/articles/images/Previous.png)](01_search_overview_and_use_cases.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](03_creating_elasticsearch_indexes_on_search_fields.md)