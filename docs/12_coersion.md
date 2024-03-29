## Coersion
Coercion is the process of converting values to match the expected data type defined in the mapping.

- When indexing documents, Elasticsearch checks the data type specified in the mapping for each field.
- If a value does not match the defined data type, Elasticsearch attempts to coerce it to match.
- Coercion rules vary based on the target data type. For example, string values may be coerced to numbers, dates, or boolean values based on their format.
- If coercion fails (e.g., attempting to coerce a non-numeric string to a number), Elasticsearch may reject the document or apply a default value.

### DSL for Mapping with Coercion:
```json
PUT /products
{
  "mappings": {
    "dynamic": "true",
    "properties": {
      "age_group": {
        "type": "keyword"
      },
      "price": {
        "type": "float"
      },
      "description": {
        "type": "text",
        "coerce": false
      }
    }
  }
}
```
Suppose we're indexing product data:

```json
PUT /products/_doc/1
{
  "age_group": "adult",
  "price": "199.99",
  "description": "Premium quality product for adults"
}
```
The document will be index correctly with coersion on price (to float).

Now, consider adding another record,
```json
PUT /products/_doc/2
{
  "age_group": "18+",
  "price": "199.99",
  "description": 25
}
```
For description, Elasticsearch will fail to coerce the invalid long value to a string because coerse is disabled. The indexing operation will fail, resulting in an error.
