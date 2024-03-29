## CRUD operations:

### Create (Index) Document

```bash
PUT /index_name/_doc/document_id
{
  "field1": "value1",
  "field2": "value2"
}
```
Example:
```bash
PUT /products/_doc/1
{
  "name": "Product 1",
  "price": 100
}
```

### Read (Get) Document

```bash
GET /index_name/_doc/document_id
```
Example:
```bash
GET /products/_doc/1
```

### Update Document

```bash
POST /index_name/_update/document_id
{
  "doc": {
    "field_to_update": "new_value"
  }
}
```
Example:
```bash
POST /products/_update/1
{
  "doc": {
    "price": 120
  }
}
```

### Delete Document

```bash
DELETE /index_name/_doc/document_id
```
Example:
```bash
DELETE /products/_doc/1
```

### Bulk Operations

Bulk index, update, or delete multiple documents in a single request.

```bash
POST /index_name/_bulk      // Specifies the HTTP method and target index for the bulk request

// Action: Index a new document with ID "1"
{"index":{"_id":"1"}}       // Specifies the action type ("index") and document ID ("1")
{"text_field":"This is a sample text field."}  // Document data to be indexed

// Action: Create a new document with ID "2"
{"create":{"_id":"2"}}      // Specifies the action type ("create") and document ID ("2")
{"keyword_field":"category1"} // Document data to be created

// Action: Update an existing document with ID "3"
{"update":{"_id":"3"}}      // Specifies the action type ("update") and document ID ("3")
{"doc":{"integer_field":42}} // Partial document data to be updated

// Action: Delete an document with ID "3"
{"delete":{"_id":"3"}}      // Delete document with ID "3"
```

### Search Documents

```bash
GET /index_name/_search
{
  "query": {
    "match": {
      "field": "value"
    }
  }
}
```
Example:
```bash
GET /products/_search
{
  "query": {
    "match": {
      "name": "Product"
    }
  }
}
```

### Check if Document Exists

```bash
HEAD /index_name/_doc/document_id
```
Example:
```bash
HEAD /products/_doc/1
```
This returns a `200 OK` status code if the document exists or a `404 Not Found` status code if it does not.

### Retrieve Document if Exists

```bash
GET /index_name/_doc/document_id?_source
```
Example:
```bash
GET /products/_doc/1?_source
```
This returns the document along with a `200 OK` status code if the document exists, or a `404 Not Found` status code if it does not.

### `update_by_query`
- The update_by_query API allows you to update documents in an index based on a specified query.
- It applies a script to each matching document, enabling you to modify specific fields or values in bulk.
- This can be useful for making widespread updates to your data without needing to retrieve and update each document individually.

**DSL**:
```json
POST /my_index/_update_by_query
{
  "script": {
    "source": "ctx._source.age += params.value", // Alternatively, ctx._source.age++
    "params": {
      "value": 10
    }
  },
  "query": {
    "term": {
      "category": "books"
    }
  }
}
```

**Use Case**:
- **Scenario**: You want to update documents in your index that match a specific condition.
- **Use Case**: Update all documents where the "category" field is "books" by incrementing the value of a particular field.

### `delete_by_query`
- The delete_by_query API enables you to delete documents from an index based on a specified query.
- It identifies all documents that match the query criteria and removes them from the index.
- This operation is helpful for removing unwanted or outdated data from your Elasticsearch index in bulk.

**DSL**:
```json
POST /my_index/_delete_by_query
{
  "query": {
    "term": {
      "category": "books"
    }
  }
}
```

**Use Case**:
- **Scenario**: You need to delete documents from your index that match certain criteria.
- **Use Case**: Delete all documents where the "category" field is "books".
