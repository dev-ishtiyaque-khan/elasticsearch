## Reindex

Reindexing in Elasticsearch involves transferring data from one index to another, often to accommodate changes in mapping or to optimize data structure. The `_reindex` API is commonly used for this purpose. Here's an explanation:

- **Reindexing**: Reindexing is the process of copying data from one index to another. It's typically done to apply changes in mapping, filter data, or upgrade Elasticsearch versions.
- **_reindex API**: Elasticsearch provides the `_reindex` API, which allows users to copy documents from a source index to a destination index.
- **Usage**: 
  - The `_reindex` API requires specifying the source index and optionally a query to filter documents.
  - Users can also define a destination index where the documents will be copied.
  - The API supports various parameters for controlling the reindexing process, such as batch size, script execution, and conflicts resolution.
- **Example**: 
  ```
  POST _reindex
  {
    "source": {
      "index": "source_index",
      "query": {           // Query option to filter documents from the source index
        "match": {
          "category": "electronics"
        }
      },
      "_source": ["name", "price"]  // _source option to specify which fields to copy from the source documents
    },
    "dest": {
      "index": "destination_index",
      "script": {         // Script option to perform transformations on documents during reindexing
        "source": "ctx._source.price *= params.rate",
        "params": {
          "rate": 1.1
        }
      }
    }
  }
  ```
