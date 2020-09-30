# Search Command

The **SEARCH** command runs a search on the Elasticsearch for specific LU tables and columns and returns a result set with the search results.

#### Usage: 

```
SEARCH lutype=<LUT_Name> TABLES=<tables names> '<Search Query>';   
```

- The TABLES parameter can be populated by one or several LU tables, separated a comma. It is possible to include several LU tables in one search only if these tables have the same list of search fields (search indexes).       
- The **Search Query** parameter is populated by a JSON with Elasticsearch query.
- The **SEARCH** command returns the records that match the search query. The following information is displayed for each record:
  - The LUI of each record.
  - The list of Search fields, defined in the specified LU table.
  - The score returned by the Elasticsearch.

### Search Command- Examples



<table>
<tbody>
<tr>
<td valign="top" width="100pxl"><strong>Use Case</strong></td>
<td valign="top" width="150pxl"><strong>Example</strong></td>
<td valign="top" width="100pxl"><strong>Search Index Type</strong></td>
<td valign="top" width="200pxl"><strong>Implementation Guidelines</strong></td>
<td valign="top" width="100pxl"><strong>ES method</strong></td>
<td valign="top" width="450pxl"><strong>Search Example</strong></td>
</tr>
      <tr>
<td valign="top" width="100pxl">Pattern Matching</td>
<td valign="top" width="150pxl">Tal* = Tali</td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="200pxl">&nbsp;</td>
<td valign="top" width="100pxl">query_string</td>
<td valign="top" width="450pxl">
<ul>
<li>Search lutype=CUSTOMER table=CUSTOMER '{ "query": { "query_string": { "fields": ["FIRST_NAME"], "query": "Tal*" } } }</li>
</ul>
</td>
</tr>
    <td valign="top" width="100pxl">Identical values- case sensitive check</td>
<td valign="top" width="150pxl">Tali = Tali</td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="200pxl">&nbsp;</td>
<td valign="top" width="100pxl">match/match_phrase</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match_phrase" : { "FULL_NAME": {"query" : "Waneta Hensley"}}}}';</li>
</ul>
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match" : { "MAIL.keyword": {"query" : "tali.einhorn@k2view.com"}}}}';</li>
</ul>
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match" : { "CUSTOMER_ID": {"query" : "20"}}}}';</li>
</ul>
</td>
</tr>
    <td valign="top" width="100pxl">Identical values including special characters- case insensitive check</td>
<td valign="top" width="150pxl">Tali = tali = TALI&nbsp;</td>
<td valign="top" width="100pxl">case-insensitive-match</td>
<td valign="top" width="200pxl">Add <strong>case-insensitive-match</strong> template to the search options in the .k2proj file and select it as the type for the search field</td>
<td valign="top" width="100pxl">match</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": { "match": { "FIRST_NAME": "chris" }}}</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="100pxl">Fuzzy search, ignore spelling errors.</td>
<td valign="top" width="150pxl">
<ul>
    <li>Ludwigshafern = Ludwigshafen</li>
	<li>12345=12346 =1234</li>
</ul>    
 </td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="200pxl">&nbsp;</td>
<td valign="top" width="100pxl">
<p>query + match + fuzziness<p/>
<p>You need to define the fuzziness parameter, i.e. how many mistakes are allowed</p>
</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {&nbsp; &nbsp;"match" : {"CITY": {&nbsp;"query": "Ludwigshafern",&nbsp;&nbsp;"fuzziness": "2" }&nbsp;}}}</li>
<li>Notes:
<ul>
<li>The fuziness parameter indicates how many mistakes are allowed (missing, extra, oe incorrect&nbsp; character).</li>
<li>A swap of two adjacent characters- for example: 'Tali' and 'Tail'- is considered as one error.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="100pxl">Fuzzy search- mail</td>
<td valign="top" width="150pxl">tali.einhorn@k2view.com = TALI.EINHORN@k2view.com = tali.einhon@k2view.com<br /> <br /> tali.einhorn@k2view.com != tali.einhorn@yahoo.com</td>
<td valign="top" width="100pxl">case-insensitive-match</td>
<td valign="top" width="200pxl">
    <p> Add a <strong>case-insensitive-match</strong> template to the search options in the .k2proj file and select it as the type for the search field. This setting is needed to enable case insensitive searches on the whole mail address. By default (text) - the @ and . chars split the tokens, so if you use the default setting, the search command matches "tali@gmail.com" to "tali.k2view.com". </p></td>
<td valign="top" width="100pxl">match + fuzziness</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match" : { "MAIL": {"query" : "tali.einhon@k2view.com", "fuzziness": "1"}}}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="100pxl">Partial match- ignore additional information</td>
<td valign="top" width="150pxl">Frankfurt/Main' matches&nbsp; 'Frankfurt am Main'<br /> 'Charlottenburg bei Berlin' matches 'Berlin-Charlottenburg, F&uuml;rth/Bay'</td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="200pxl">Works for text fields only. If the field is defined as Integer in the implementation, the search does not work.</td>
<td valign="top" width="100pxl">Options:<br /> 1. match_phrase + slop. The value of the slop defines the number of words that can be located between the words of the search. The default of the slop is zero.<br /> 2 .match for each one of the required words <br /> Note- unlike the match- the match_phrase does not support using fuzziness. To use fuzziness- we need to run search with several match commands- one for each word + fuzziness</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match_phrase" : { "ADDRESS": {"query" : "Washington Rd", "slop": "2"}}}}';</li>
</ul>
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"bool": {"must": [{"match" : { "ADDRESS": {"query" : "Washington", "fuzziness": "1"}}},{"match" : { "ADDRESS": {"query" : "Rd", "fuzziness": "1"}}}]}}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="100pxl">Ignore leading or trailing blanks</td>
<td valign="top" width="150pxl">1234 ' = ' 1234' = ' 1234 '</td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="200pxl">
    <p>Works for Text fields only.</p></td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match" : { "FIRST_NAME": {"query" : "Waneta"}}}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="100pxl">Transposed digits- partial match- at least x characters must exist. At least y% of them must have the right order</td>
<td valign="top" width="150pxl">At least 4 digits must exist and at least 50% of them must be in the right order-<br /> '1243'/ '56347712'/'654331234' match '1234'.<br /> '7777712' and '8751883724' do not match '1234'&nbsp; <br /> 123456=1246= 9230056=1243</td>
<td valign="top" width="100pxl">precision-match-20</td>
<td valign="top" width="200pxl">Add <strong>precision-match-20</strong> template to the search options in the .k2proj file and select it as the type for the search field</td>
<td valign="top" width="100pxl">bool + must</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=ACCOUNT tables=ACCOUNT '{"query":{"bool":{ "must":[{"match":{"ACCOUNT_NUMBER.1gram":{ "query":"123456",&nbsp;"minimum_should_match":4}}},{"match":{"ACCOUNT_NUMBER.2gram":{"query":"123456","minimum_should_match":1 }}} ]}}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="100pxl">Transposed digits- partial match- at least x characters must exist. At least y% of them must have the right order</td>
<td valign="top" width="150pxl">At least 6 digits must exist and at least 3 of them must be in the right order-<br /> '1243'/ '56347712'/'654331234' match '1234'.<br /> '7777712' and '8751883724' do not match '1234'&nbsp;&nbsp;</td>
<td valign="top" width="100pxl">precision-match-20</td>
<td valign="top" width="200pxl">Add <strong>precision-match-20</strong> template to the search options in the .k2proj file and select it as the type for the search field</td>
<td valign="top" width="100pxl">bool + must</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=ACCOUNT tables=ACCOUNT '{"query":{"bool":{"must":[ {"match":{"ACCOUNT_NUMBER.1gram":{"query":"86422233", "minimum_should_match":6 }}}, {"match":{"ACCOUNT_NUMBER.3gram":{"query":"86422233", "minimum_should_match":1&nbsp;}}} ] }}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
    <p>At least 4 digits exist in the searched field, but the searched field must not have more than two additional digits o letters.</p></td>
<td valign="top" width="150pxl">
    <ul>
        <li>'123456'= '1234'= '1234 AB'= '3456' = '345699'</li>
        <li>'123456' != '1234567' != '3456890' != '34599'</li>
    </ul>
   </td>
<td valign="top" width="100pxl">precision-match-20</td>
<td valign="top" width="200pxl">Add <strong>precision-match-20</strong> template to the search options in the .k2proj file and select it as the type for the search field</td>
<td valign="top" width="100pxl">
    <p>bool + musy + fuzzy</p>
    <p>Note that a fuzzy search supports up to two errors.</p></td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=ACCOUNT tables=ACCOUNT '{ "query":{ "bool":{"must":[{ "match":{"ACCOUNT_NUMBER.1gram":{"query":"123456", "minimum_should_match":4}}}, { "fuzzy":{"ACCOUNT_NUMBER":{"value":"123456", "fuzziness":"2", transpositions":false }}} ] }}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="200pxl"><p>
    At least 6 digits exist in the searched field, but the seached field must not have more than two missing digits or letters.</p></td>
<td valign="top" width="150pxl">&gt; '12345678'= '123456' = '345679'<br /> &gt; '123456890' = '12345688'<br /> &gt; '12345678' != '12345000'<br /> &gt; '876543212' != '876543'</td>
<td>precision-match-20</td>
<td valign="top" width="100pxl">Add <strong>precision-match-20</strong> template to the search options in the .k2proj file and select it as the type for the search field</td>
<td valign="top" width="100pxl">bool + should + fuzzy</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=ACCOUNT tables=ACCOUNT '{ "query":{"bool":{"should":[ { "match":{ "ACCOUNT_NUMBER.1gram":{"query":"12345678", "minimum_should_match":6 }}} {"match":{"ACCOUNT_NUMBER.1gram":{"query":"12345678", "minimum_should_match":7 }}} {"match":{"ACCOUNT_NUMBER.1gram":{"query":"12345678", "minimum_should_match":8}}}{"fuzzy":{"ACCOUNT_NUMBER":{"value":"12345678", "fuzziness":"2", "transpositions":false }}} ] }}}';</li>
</ul>
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td>Only one digits is different in the searched field</td>
<td valign="top" width="150pxl">Searched value: '12345'-<br /> Return the following values: '12346', '15345'<br /> Do not return '12366'</td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">query + fuzzy</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"fuzzy" : {"ZIP": {"value": "70451", "fuzziness": 1, "transpositions": false }}}}';</li>
</ul>
<ul>
<li>search&nbsp;lutype=CUSTOMER tables=CUSTOMER '{"query": { "fuzzy" : { "NAME": { "value": "Tali", "fuzziness": 1, "transpositions": false } } }}';</li>
<li>Note that when the transposition parameter is set to false, two adjacent characters are considered as two mistakes. By default- this parameter is set to true.</li>
</ul>
</td>
</tr>
<tr>
<td>Check several fields for match</td>
<td valign="top" width="150pxl">&gt; Check name + location<br /> &gt; NAME = 'EINHORN' and GIVEN_NAME = 'TALI' and BIRTH_DATE is null and POSTAL_CODE = '63462'<br /> &gt; Check Register_type + register_number+ register_city + location&nbsp;</td>
<td valign="top" width="100pxl">keyword/date</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">boolean match- bool + match or bool + query_string</td>
<td valign="top" width="100pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"bool": {"must": [{"match" : { "FIRST_NAME": {"query" : "Marget", "fuzziness": "1"}}},{"match" : {"LAST_NAME":&nbsp; {"query" : "Lane", "fuzziness": "1"}}}, {"match": {"CITY": {"query" : "ROSCO", "fuzziness": "1"}}}, {"match": {"DATE1.date": {"query" : "2018-09-14"}}}]}}}';</li>
</ul>
</td>
</tr>
<tr>
<td>Support special characters- case insensitive</td>
<td valign="top" width="150pxl">David&amp;co = david&amp;co = davi@Co</td>
<td valign="top" width="100pxl">case-insensitive-match</td>
<td valign="top" width="100pxl">Add <strong>case-insensitive-match</strong> template to the search options in the .k2proj file and select it as the type for the search field. This setting is needed to enable case insensitive search on the whole mail address. By default (text) - the @ and . chars split the tokens, so if you use the default setting, the search command matches "David" to "David@Co".</td>
<td valign="top" width="100pxl">match</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match" : { "FIRST_NAME": {"query" : "Davi&amp;co", "fuzziness": "1"}}}}';</li>
</ul>
</td>
</tr>
<td>Support scoring for matching</td>
<td valign="top" width="150pxl">Full match: 'Ludwigshafern' = 'Ludwigshafern' =&gt; gets scoring of 1<br /> Ignore spelling errors or additional information: 'Ludwigshafern' = 'Ludwigshafen', 'Charlottenburg bei Berlin' = 'Berlin-Charlottenburg' =&gt; gets scoring of 0.85 <br /> Partial match: "1234 AB" =&nbsp; "1234" = "123456"&nbsp; =&gt; gets scoring of 0.9&nbsp;</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query":{"dis_max":{"queries":[ {"function_score":{"query":{"match":{"MAIL":"SyhKHckZ@gmail.com"}},"script_score":{"script":{"source":"0.9"}}, "score_mode":"max","boost_mode":"replace"}},{"function_score":{ "query":{ "match":{"MAIL":"SyhKHckZ@gmail.com"}}, "script_score":{ "script":{ "source":"1"}}, "score_mode":"max", "boost_mode":"replace"}}, {"function_score":{"query":{"match":{"MAIL":{"query": "SyhKHckZ@gmail.com", "fuzziness": 2}}}, "script_score":{"script":{"source":"0.85" }}, "score_mode":"max", "boost_mode":"replace" }} ] }}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">Support predictive search</td>
<td valign="top" width="150pxl">When the user types 'Ber' =&gt; complete the value to 'Berlin'</td>
<td valign="top" width="100pxl">predictive-search</td>
<td valign="top" width="100pxl">Add <strong>predictive-search</strong> template to the search options in the .k2proj file and select it as the type for the search field</td>
<td valign="top" width="100pxl">bool + query_string</td>
<td valign="top" width="450pxl">
<ul>
<li>Date fields:
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"bool": {"must": [{"query_string": { "fields": ["DATE1"], "query" : "2018", "fuzziness": "1"}},{"query_string": { "fields": ["DATE1"], "query" : "09", "fuzziness": "1"}}]}}}';</li>
</ul>
</li>
</ul>
<ul>
<li>Text fields:
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"bool": {"must": [{"query_string": { "fields": ["ADDRESS"], "query" : "Washington", "fuzziness": "1"}},{"query_string": { "fields": ["ADDRESS"], "query" : "R*", "fuzziness": "1"}}]}}}';</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">Match multiple fields if they have crossed values. For example, the name and given name are populated in different fields</td>
<td valign="top" width="150pxl">
    <ul>
        <li>NAME = 'Tali', FAMILY_NAME = "Einhorn"</li>
        <li>NAME = 'Einhorn', FAMILY_NAME = 'Tali'</li>        
    </ul>
</td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">
    <p>bool + must + multi_match.</p>
    <p>Note: the difference between should and must: must- all conditions must match. Should- you can set a parameter - minimum_should_match- how many conditions needs to match</p>
</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"bool": {"must": [{ "multi_match": { "query": "Tali", "fields": ["NAME", "FAMILY_NAME"] } },<br /> { "multi_match": { "query": "Einhorn", "fields": ["NAME", "FAMILY_NAME"] } } ] }}}';</li>
</ul>
<p>&nbsp;</p>
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"bool": {"must": [{"multi_match": {"query": "Cris", "fields": ["FIRST_NAME", "LAST_NAME"]}}, {"multi_match": {"query": "Michael", "fields": ["FIRST_NAME", "LAST_NAME"]}}]}}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">Match a different order of words within a field</td>
<td valign="top" width="150pxl">Tali Einhorn'= 'Einhorn Tali'</td>
<td valign="top" width="100pxl">Keyword</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">match_phrase</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"match_phrase" : {"FULL_NAME": {"query": "Cris Michael", "slop": 2 }}}}';</li>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": { "match_phrase" : { "fullname": { "query": "Einhorn Tali", "slop": 2 }}}';</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">Check date ranges</td>
<td valign="top" width="150pxl"><p>
    The date is between 2019-01-01 to 2019-02-01.</p></td>
<td valign="top" width="100pxl">date</td>
<td valign="top" width="200pxl"><p>
    <ul>        
        <li>The date field must be kept in Fabric with a format, supported by the Elasticsearch. For example: 'YYYY-MM-DD'.</li>
        <li>See the list of date formats in the Elasticsearch web site.</li>
    </ul>
  </td>
<td valign="top" width="100pxl">range</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"range" : {"DATE1.date" : { "gt" : "2018-07-02", "lte" : "2018-08-07"}}}}';</li>
</ul>
</td>
</tr>
<tr>
<td>Check date values- check for equal dates</td>
<td valign="top" width="150pxl">2003-03-04 00:05:30' = '2003-03-04 00:05:30' = '2003/03/04 00:05:30' = '2003-MAR-04 00:05:30'<br /> '2019-01-20' = '20-JAN-2019' = '2019/01/20'</td>
<td valign="top" width="100pxl">date</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">query_string</td>
<td valign="top" width="450pxl">
<ul>
<li>search lutype=CUSTOMER tables=CUSTOMER '{"query": {"query_string": { "fields": ["DATE1.date"], "query": "2018-08-02"}}}';</li>
</ul>
</td>
</tr>
<tr>
<td>Check for empty string</td>
<td valign="top" width="150pxl">"Tali" = ""</td>
<td valign="top" width="100pxl">keyword</td>
<td valign="top" width="100pxl">Empty String are kept as null values in the ES</td>
<td valign="top" width="100pxl">bool + should + must_not + exist</td>
<td valign="top" width="450pxl">
<ul>
<li>Date field:
<ul>
<li>search lutype=CUSTOMER_ES tables=VISITS '{"query": {"bool": {"should": [{"bool": {"must_not": {"exists": {"field": "VISIT_DATE"}}}}, {"match": {"VISIT_DATE.date": {"query": "2008-02-29"}}}]}}}';</li>
</ul>
</li>
</ul>
<p>&nbsp;</p>
<ul>
<li>Keyword field:
<ul>
<li>search lutype=CUSTOMER_ES tables=CUSTOMER '{"query": {"bool": {"should": [{"bool": {"must_not": {"exists": {"field": "CITY"}}}}, {"match": {"CITY": {"query": "BABIT", "fuzziness": "2"}}}]}}}';</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr>
<td>Check for null values</td>
<td valign="top" width="150pxl">birth_date may be empty for some of the customer and populated for others. Do we need to avoid including the birth_Date in the search if its value is null?</td>
<td valign="top" width="100pxl">keyword/date</td>
<td valign="top" width="100pxl">&nbsp;</td>
<td valign="top" width="100pxl">bool + should + must_not + exist</td>
<td valign="top" width="450pxl">
<ul>
<li>Date field:
<ul>
<li>search lutype=CUSTOMER_ES tables=VISITS '{"query": {"bool": {"should": [{"bool": {"must_not": {"exists": {"field": "VISIT_DATE"}}}}, {"match": {"VISIT_DATE.date": {"query": "2008-02-29"}}}]}}}';</li>
</ul>
</li>
</ul>
<ul>
<li>keyword field:
<ul>
<li>search lutype=CUSTOMER_ES tables=CUSTOMER '{"query": {"bool": {"should": [{"bool": {"must_not": {"exists": {"field": "CITY"}}}}, {"match": {"CITY": {"query": "BABIT", "fuzziness": "2"}}}]}}}';</li>
</ul>
</li>
</ul>
</td>
</tr>
</tbody>
</table>







