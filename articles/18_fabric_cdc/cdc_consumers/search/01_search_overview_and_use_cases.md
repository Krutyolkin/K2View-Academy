# Search Overview and Use Cases

Fabric CDC solution has a built-in integration with Elasticsearch to enable a cross-instance search. 

For example, search all customers named “John Doe” and live in “New-York”.

The search is done on LUIs if the LU has [Search indexes]().

The deploy of the LU creates indexes in the Elasticsearch, and every data change on the LUIs updates the data in the Elasticsearch for the predefined Search indexes.

Therefore the LUIs must be loaded to Fabric to enable a cross instance search on them. For example, the search of all customers named “John Doe” and live in “New-York” will only return customers that exist in Fabric.

The search can be executed either by running Search request directly on the Elasticsearch, or by [Fabric Search Commands]. 

### Search Use Cases

1. Matching, search all LUIs by a searched criteria. 

   - Examples:
     - Search all customers whose first name is "Meryl" and last name is  "Streep". Run case insensitive search and allow up to two errors. The search returns customers named "Meryl Streep", "meryl streep", or "Meril Streep" .
     - Search all customer whose full name is "Meryl Streep". Enable a different order of words within a field. The search returns customer named "Meryl Streep" or "Streep Meryl".
     - Search customers whose ID number is '1234567'. Return all customer whose ID number contains at least 4 digits of the searched value and at least 2 of the digits have the correct order. The search returns '1243',  '56347712', and '654331234'.
     - Support partial match.  At least 8 digits exist, but there must not be more than 2 additional digits/letters.

2. Predictive search. Complete the value, typed by the user. 

   - Example: when the user types "Ber",  complete the value to "Berlin" or "Berry".

   [Click here for more information about Search examples].

   



[<img align="right" width="60" height="54" src="/articles/images/Next.png">](02_search_implementation.md)

