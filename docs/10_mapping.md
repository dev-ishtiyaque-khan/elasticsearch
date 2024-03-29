## Mapping
- Mapping defines the schema or structure of documents in an index, specifying the data types and properties of fields.
- Mapping ensures data consistency by defining the expected structure of documents, controls data indexing and querying for optimized search performance, and facilitates accurate data analysis and aggregation through defined field properties.
- **Example:** If an index contains product documents, mapping defines fields such as "name" (text), "price" (float), and "category" (keyword), ensuring consistent data representation.

### Dynamic Mapping:
Dynamic mapping settings in Elasticsearch determine how fields are dynamically mapped during indexing:
- **Dynamic True** enables Elasticsearch to create mappings dynamically for fields not explicitly defined.
- **Dynamic False** disables dynamic mapping, necessitating explicit field definitions in the mapping.
- **Dynamic Strict** rejects documents with unmapped fields, enforcing strict schema adherence.

### Defining Mapping for an Index:
- To define mapping for an index, you can use the following simple JSON format:

```json
PUT /my_index
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "price": {
        "type": "float"
      },
      "category": {
        "type": "keyword"
      }
    }
  }
}
```

### Retrieving Index Mapping:
To retrieve the mapping of an index named "my_index," you can use the following API request:
```json
GET /my_index/_mapping
```
This API call returns the mapping definition of all fields in the specified index.

### Retrieving Field Mapping:
To retrieve the mapping of a specific field, such as the "name" field in "my_index," you can use the following API request:
```json
GET /my_index/_mapping/field/name
```
This API call returns the mapping definition of the "name" field in the specified index.

### Adding Mapping for a New Field:
To add mapping for a new field, you can update the mapping definition of the index using the following API request:
```json
PUT /my_index/_mapping
{
  "properties": {
    "new_field": {
      "type": "text"
    }
  }
}
```
This API call adds mapping for a new field named "new_field" with the specified data type (e.g., "text").

### Update Mapping:

In Elasticsearch, it's not directly possible to update the mapping of an existing field once it's defined. However, you can add new fields to the mapping or reindex data with updated mapping. Alternatively, you can create a new index with the desired mapping and reindex your data into the new index.
