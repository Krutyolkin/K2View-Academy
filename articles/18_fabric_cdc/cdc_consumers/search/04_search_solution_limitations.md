# Search Solution- Limitations

- Each search can include only one LU.  
- A search can include several LU tables only if they have the same structure and the same list of search fields. Therefore if it is required to run a search by fields, related to different LU tables, it is recommended to build an additional LU table that contains all the search fields that need to be included in one search command.
- The column included in the search must be defined in Fabric  in capital letters.
- The implementation must define the search field as TEXT in the LU table to enable fuzzy search, ignore spaces, and case insensitive searches.
- A Date field must be populated in Fabric with a format, supported by the Elasticsearch. For example- ‘YYYY-MM-DD’.
- Non-english languages are currently not supported.
- Reset drop command does not clean the Elasticsearch. You must run the [DROP LU](/articles/02_fabric_architecture/04_fabric_commands.md#drop-lu-command) command to clean each one of your LUs from the Elasticsearch.
- Note that Fabric Studio does not enable defining more than 63 columns as a CDC field in the same LU table, assuming that all columns are positioned according to 1 to 63 in the LU table.