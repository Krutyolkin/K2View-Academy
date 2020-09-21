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

## Creating Search Indexes

It is required to define Search indexes on the selected LU tables' columns to enable a cross-instance search based on these columns. For example, to enable a search of all customers called "John Doe", it is required to define FIRST_NAME and LAST_NAME columns of the customer schema as Search Indexes.

To create a Search index on a column, do either: 

- Right click the selected column > **Create Search Index From Selected Columns** > select the index type.
- Open the **Search** tab populate the **Column Name** and **Type**. 

Note that Fabric Studio does not enable defining more than 63 columns in a CDC field in the same LU table, assuming that all columns are positioned according to 1 to 63 in the LU table.

### Search Index Types

The default built-in types of the Search Indexes are:

- **keyword**, enables a search by this column. Behind the scenes, Fabric creates two Search Indexes for the keyword field: keyword index and additional index based on the type of the field. The keyword index enables performing a search by exact match (case sensitive) to the searched value. 
- **date**, enables a search on a date column.  Date fields must be populated by a date format, identified by the Elastic Search. See the list of the date formats, supported by the Elastic Search:
  * [https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#built-in-date-formats](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#built-in-date-formats)
- **data**, can be returned by the search, but you cannot initiate a search by this column. 

**Example:** 

- Set the Search Indexes of Customer LU as follows:
  - First Name and Last Name are keyword fields.
  - Customer ID is a data field.
- You can run a search to get the list of all customer IDs called “John Doe”. 

### Search Index Types' Templates

-  Fabric supports adding templates for search indexes when the default settings do not match your search needs. For example: support case insensitive search by special characters (search by  mail address).

- Each template has a JSON format and creates special index settings in the Elasticsearch.
- All templates must be located on the Fabric server under the following directory:
  -  $K2_HOME/config/cdc-tags
- Fabric provides  **default templates** under the **cdc-tags** directory:

<table width="900pxl">
<tbody>
<tr>
<td width="150pxl">
<p><strong>Template Name</strong></p>
</td>
<td width="150pxl">
<p><strong>Elasticsearch Analyzer or Tokenizer</strong></p>
</td>
<td valign="top" width="300pxl">
<p><strong>Use Case</strong></p>
</td>
<td valign="top" width="300pxl">
<p><strong>Example</strong></p>
</td>
</tr>
<tr>
<td width="150pxl">
<p>case-insensitive-match.json</p>
</td>
<td width="150pxl">
<p>keyword + lowercase</p>
</td>
<td valign="top" width="300pxl">
<p>Case insensitive search of special characters. For example- search by mail address.</p>
</td>
<td valign="top" width="300pxl">
<ul>
<li>JohnD@gmail.com matches johnd@gmail.com</li>
<li>JohnD@gmail does not match JohnD@yahoo.com</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="150pxl">
<p>predictive-search.json</p>
</td>
<td valign="top" width="150pxl">
<p>edge_ngram</p>
</td>
<td valign="top" width="300pxl">
<p>Predictive search.</p>
</td>
<td valign="top" width="300pxl">
<ul>
<li>Typing "s" returns "search", "see", or "sql".</li>
<li>Typing "se" returns "search" or "see".</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="150pxl">
<p>precision-match-20.json</p>
</td>
<td valign="top" width="150pxl">
<p>1gram &ndash; 20gram</p>
</td>
<td valign="top" width="300pxl">
<p>Partial match. For example- at least 4 digits must exist and at least 50% of them must be in the right order in the searched value.</p>
<p>This template supports a maximal length of 20 chars/digits in the searched value.</p>
</td>
<td valign="top" width="300pxl">
<ul>
<li>"1243", "56347712", or "654331234" matches "1234".</li>
<li>"7777712" or "8751883724" does not match "1234".</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Adding the Templates to the Types List for Search Columns

1. Go tp the project tree, right-click the project name >  **Open Folder**. 

2. Open the [project name].k2proj file for editing.

3. Edit the **Search** options under the **DataChangeIndicators** tag and add the template names to the Options tag. See below an example for adding **precision-match-20 template:

   ```
   <DataChangeIndicators>
       <DataChange name="Search" enabled="true">
         <Options>
           <option>keyword</option>
           <option>data</option>
           <option>date</option>
           <option>precision-match-20</option>
         </Options>
   ```

   

4. Save and close the. k2proj file.

5. Close and reopen your project to reload the updated . k2proj file.

6. When you create a new Search index, you can also select the added template as a **Type**.

   

#### Update Search Columns Using Templates

- Fabric only supports using templates on Search indexes when setting the template as a type on the first deploy of the corresponding LU table. This is because Fabric creates an index in the Elasticsearch for each LU Table that has Search indexes. Using a template on a search field requires different index settings in the Elasticsearch.  Since the index settings cannot be changed once created in the Elasticsearch, an update of the search field from a supported Elasticsearch type to a template requires the following working procedure:

  - Getting the index definition from the Elasticsearch using Elasticsearch API.
  - Removing the index definition from the Elasticsearch using Elasticsearch API.
  - Creating the updated index in the Elasticsearch using Elasticsearch API. 
  - Initiating a [batch process] to run CDC_REPUBLISH_INSTANCE to republish the data of each LUI to the Elasticsearch.

- Notes:

  - Updating the Search index type in the Fabric Studio and redeploying the LU does not re-create the Search index in the Elasticsearch, since Fabric already has the LU table and therefore identifies this change as schema update instead of creating a new index in the Elasticsearch.
  - Removing and re-adding the LU table from Fabric is not recommended, since the data of the table might be lost for all the LU instances, that already exist in Fabric. For example:
    - Remove Address LU table from Customer LU and redeploy the updated Customer LU to Fabric.
    - Re-add the Address LU table into the Customer LU and redeploy the updated LU to Fabric.
    - The Address LU table is added as empty table. You must re-sync the LUIs to get the Address data for them. 

  

[![Previous](/articles/images/Previous.png)](03_cdc_implementation_steps.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](05_cdc_other_consumers_implementation.md)