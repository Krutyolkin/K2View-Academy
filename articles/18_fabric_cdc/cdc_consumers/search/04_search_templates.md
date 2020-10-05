### Search Index Types' Templates

-  Fabric supports adding templates for search fields when the default settings do not match your search needs. For example: search by fields that contain special characters like a mail address.

-  Each template has a JSON format and creates special index settings in the Elasticsearch.
-  All templates must be located on the Fabric server under the following directory:
   -  $K2_HOME/config/cdc-tags
-  Fabric provides  **default templates** under the **cdc-tags** directory:

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

3. Edit the **Search** options under the **DataChangeIndicators** tag and add the template names to the Options tag. See below an example for adding **precision-match-20 template**:

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

- Fabric only supports using templates on Search fields when setting the template as a type on the first deploy of the corresponding LU table. This is because Fabric creates an index in the Elasticsearch for each LU Table that has Search indexes. Using a template on a search field requires different index settings in the Elasticsearch.  Since the index settings cannot be changed once created in the Elasticsearch, an update of the search field from a supported Elasticsearch type to a template requires the following working procedure:

  - Getting the index definition from the Elasticsearch using Elasticsearch API.
  - Removing the index definition from the Elasticsearch using Elasticsearch API.
  - Creating the updated index in the Elasticsearch using Elasticsearch API. 
  - Initiating a [batch process](/articles/20_jobs_and_batch_services/11_batch_process_overview.md) to run CDC_REPUBLISH_INSTANCE to republish the data of each LUI to the Elasticsearch.

  [Click for more information about creating the Elasticsearch indexes](03_creating_elasticsearch_indexes_on_search_fields.md).

  

  [![Previous](/articles/images/Previous.png)](03_creating_elasticsearch_indexes_on_search_fields.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](05_search_command.md)
