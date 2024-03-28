## Key Concept

1. **Node**:
   - A single instance of Elasticsearch running on a physical or virtual machine.
   - Nodes can be configured to perform specific roles within a cluster, such as data node, master-eligible node, or coordinating node.

2. **Cluster**:
   - A collection of one or more nodes that together holds the entire data and provides federated indexing and search capabilities across all nodes.
   - Clusters provide scalability, reliability, and distributed computing capabilities for Elasticsearch.

3. **Index**:
   - A collection of documents that share similar characteristics and are stored and indexed together.
   - Indices are used to organize and retrieve documents efficiently, typically representing a single data type or source.

4. **Fields**:
   - Individual components of a document representing specific attributes or properties of the data.
   - Fields can be of different data types such as text, keyword, numeric, date, etc., and are used for indexing, searching, and aggregating data.

5. **Document**:
   - The basic unit of information stored in Elasticsearch, represented as a JSON object.
   - Each document is stored within an index and consists of multiple fields containing data and metadata related to a specific entity or record.

6. **Sharding**:
   - Divides index into smaller parts called shards for better scalability and performance.
   - Enables distribution of data across multiple nodes, improving parallelism and search speed.
   - Sharding in Elasticsearch is like dividing a big book into smaller chapters. Each chapter (shard) contains part of the information. By doing this, Elasticsearch can distribute the workload across multiple chapters (shards), making searches faster and the system more scalable. It's like having many people reading different chapters at the same time, making progress quicker.
      ```
      Elasticsearch Cluster
         |
         ├── Node 1
         │     ├── Shard 1
         │     ├── Shard 2
         │     └── Shard 3
         │
         ├── Node 2
               ├── Shard 4
               ├── Shard 5
               └── Shard 6
      ```
7. **Replication**:
   - Creates duplicate copies of shards (replicas) for fault tolerance and high availability.
   - Provides backup copies of data, reducing the risk of data loss and ensuring resilience against node failures.
   - Replication in Elasticsearch is like making backup copies of your data. Each piece of data (shard) has a primary copy and one or more backup copies (replicas). If the primary copy is lost, the system automatically promotes a replica to take its place, ensuring data availability and resilience.
   - Operations in Elasticsearch are initially performed on the primary shard of an index. Once the primary shard processes the operation successfully, it is then replicated to the replica shards. This process ensures data consistency and fault tolerance across the cluster.
      ```
         Primary Shard     Replica Shard
         (Node 1)          (Node 2)
      ```

## Nodes Roles:

1. **Data Node**:
   - Stores data and executes data-related operations like CRUD, search, and aggregation.
   - Responsible for holding shards and handling data-intensive tasks.

2. **Master Node**:
   - Controls cluster-wide operations such as creating or deleting indices, allocating shards, and maintaining cluster state.
   - Monitors the health of nodes and manages cluster configuration.

3. **Ingest Node**:
   - Specialized node type designed for pre-processing documents before indexing.
   - Allows performing transformations, enrichments, and routing of documents before they are indexed.

4. **Client Node**:
   - Primarily used for handling client requests and serving as a gateway to the cluster.
   - Doesn't hold any data but forwards requests to appropriate data nodes for execution.

5. **Coordinating Node**:
   - Acts as a traffic cop, coordinating and distributing search and index requests across the cluster.
   - Helps in balancing the load and improving search performance by parallelizing operations.

## How to create multiple nodes in single system?

1. **Set Up Node 1:** In the `elasticsearch.yml` file, specify the settings for the first node. You'll need to configure the following settings:
   ```
   node.name: node-1
   cluster.name: my-cluster
   path.data: /path/to/your/data1
   http.port: 9200
   ```

   Adjust the `node.name`, `cluster.name`, `path.data`, and `http.port` as needed. Ensure that `http.port` is set to the default Elasticsearch port `9200`.

2. **Set Up Node 2:** Below the settings for Node 1, add configurations for Node 2. Adjust the settings accordingly:
   ```
   node.name: node-2
   cluster.name: my-cluster
   path.data: /path/to/your/data2
   http.port: 9201
   ```

   Ensure that the `node.name` is unique, and set a different `http.port` for Node 2 (e.g., `9201`).

3. **Start Elasticsearch:** Open a terminal or command prompt, navigate to the Elasticsearch installation directory, and run the Elasticsearch executable. For example:
   ```
   bin/elasticsearch
   ```

   Ensure that you start each instance of Elasticsearch separately, specifying different configurations for each node.

4. **Verify Nodes:** Once Elasticsearch is running, you can verify that both nodes are active by accessing them through their respective ports. For example:
   - Node 1: `http://localhost:9200`
   - Node 2: `http://localhost:9201`

This setup allows you to simulate a multi-node Elasticsearch cluster for testing or development purposes.

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


## Elasticsearch routing
Routing in Elasticsearch is like sorting mail into different mailboxes based on a unique key. Here's a simple explanation:
- Imagine you have a large stack of letters to organize. Each letter has a unique code written on it. Instead of randomly placing them in mailboxes, you decide to use the code to determine which mailbox they should go to.
- In Elasticsearch, documents are like letters, and routing is like assigning them to specific mailboxes (shards) based on a unique identifier (routing key). This way, documents with the same identifier are stored together in the same shard.
- So, routing in Elasticsearch is about directing documents to the right storage location based on a chosen identifier, which can improve search performance and data management.

Routing in Elasticsearch is determined by a hash function applied to a routing value. The formula for routing can be simplified as follows:

```text
shard=hash(routing_value)modnumber_of_shards
```

In this formula:

- *routing_value* is the value used to determine the routing.
- *hash* is the hash function applied to the routing value.
- *number_of_shards* is the total number of shards in the index.

## Sequence Number & Primary Term

1. **Sequence Number (Seq Number):**
- A sequence number in Elasticsearch is like a ticket for every change made to data. Each time something is added, updated, or deleted, Elasticsearch assigns a new sequence number. These numbers ensure that changes happen in the right order, especially when data is replicated or recovered.
- When an operation is executed in Elasticsearch, the sequence number increments on both the primary and replica shards, ensuring each action is uniquely identified and maintaining the correct order of operations across all shards.
- The sequence number gets updated with each operation on the document, regardless of shard assignment.

2. **Primary Term:**
- The primary term in Elasticsearch represents the version number associated with the primary shard of a document.
- This number begins at 1 and increments with each update to the document or with changes in primary shard assignment due to shard rebalancing within the cluster.
- It plays a crucial role in tracking changes and resolving conflicts that may arise during concurrent updates.

3. **Global Checkpoint:**
- The global checkpoint in Elasticsearch is like a milestone that marks the progress of data replication across all nodes in the cluster.
- It ensures that all primary and replica shards have acknowledged processing up to a certain point in the transaction log. This checkpoint is crucial during recovery processes, ensuring data consistency and integrity across the entire cluster.

Use case:
- Imagine you have an Elasticsearch cluster with multiple nodes, and you need to recover data after a node failure.
- The global checkpoint helps ensure that all nodes have replicated data up to a certain point in the transaction log.
- This allows for efficient recovery, as the cluster can resume processing from the last known consistent state.

4. **Local Checkpoint:**
- The local checkpoint in Elasticsearch is like a personal progress tracker for each shard.
- It indicates the last transaction log entry that has been applied to a particular shard, tracking its replication progress independently.
- This checkpoint is essential for monitoring and maintaining consistency within individual shards.

Use case:
- Consider a scenario where you're indexing large volumes of data into an Elasticsearch index.
- The local checkpoint helps track the progress of data replication within each shard.
- If a node fails or new replicas are added, the local checkpoint ensures that each shard knows exactly where it left off, enabling seamless recovery and replication.

## Optimistic concurrency control
- Optimistic concurrency in Elasticsearch is like a trust-based system. When multiple users try to modify the same data at once, Elasticsearch assumes everything will go smoothly. It lets everyone make their changes, but when it's time to save, Elasticsearch checks if there were any conflicts. If there were, it figures out how to resolve them, often by looking at timestamps or version numbers, ensuring data integrity without stopping everyone from working.

1. **Example**:
   - **Step 1: Retrieve Document**: Retrieve document with its sequence number and primary term.
   - **Step 2: Make Changes**: Make changes to the document.
   - **Step 3: Check Sequence Number and Primary Term**: Check sequence number and primary term again.
   - **Step 4: Update Document with Sequence Number and Primary Term**:
     ```json
     PUT /my_index/_doc/1?if_seq_no=1&if_primary_term=1
     {
       "field": "updated_value"
     }
     ```
   - **Step 5: Handle Conflicts**:
     Elasticsearch automatically handles conflicts by returning a 409 Conflict status code if the document has been updated by another process since you retrieved it. You can then handle this situation in your application logic.

## Analyser
- In Elasticsearch, an analyzer is a configurable component responsible for preprocessing and transforming text during indexing and search operations. It consists of a tokenizer and optional token filters, allowing users to customize how text is broken down into tokens and how those tokens are modified. Analyzers are crucial for tasks like stemming, stop words removal, lowercase conversion, and more, ensuring efficient and accurate search results.

### Types of Analysers:
1. Standard Analyzer: General-purpose text analysis for most languages.
- *Input:* "Quick brown foxes jumped over the lazy dog."
- *Tokens:* ["quick", "brown", "foxes", "jumped", "over", "the", "lazy", "dog"]
2. Simple Analyzer: Splits text into tokens based on non-alphabetic characters.
- *Input:* "email@example.com"
- *Tokens:* ["email", "example", "com"]
3. Whitespace Analyzer: Splits text into tokens based on whitespace.
- *Input:* "New York City"
- *Tokens:* ["New", "York", "City"]
4. English Analyzer: Tailored for English text, including stemming and stop words removal.
- *Input:* "Running dogs run faster than lazy dogs"
- *Tokens:* ["run", "dog", "run", "faster", "than", "lazi", "dog"]
5. Keyword Analyzer: Treats the entire text as a single token, useful for exact matching.
- *Input:* "HELLO World"
- *Tokens:* ["hello", "world"]
6. Lowercase Analyzer: Converts all text to lowercase.
- *Input:* "The quick brown foxes"
- *Tokens:* ["brown", "fox"]
7. Custom Analyzer: Tailored to specific requirements, combining tokenizers and filters as needed.
- *Input:* "The quick brown foxes"
- *Tokens:* ["brown", "fox"]

1. **Character Filters**: 
   Character filters are components in Elasticsearch analyzers responsible for preprocessing input text before tokenization. They can perform tasks such as HTML stripping, accent removal, or custom character mapping.

2. **Tokenization**:
   Tokenization is the process of breaking down input text into individual units called tokens. In Elasticsearch, tokenization is performed by tokenizers, which split text based on defined rules such as whitespace, punctuation, or specific characters.

3. **Token Filters**: 
   Token filters are components in Elasticsearch analyzers that modify or filter tokens generated during tokenization. They can perform tasks such as converting tokens to lowercase, removing stop words, stemming, or applying synonyms.

### Analyze DSL

```json
POST /_analyze
{
  "analyzer": "custom_analyzer",
  "text": "Your text to be analyzed goes here"
}

POST /_analyze
{
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase", "stop"],
  "text": "Your text to be analyzed goes here"
}
```
## How documents are stored in Elasticsearch?

This document provides an overview of how documents are stored in Elasticsearch, focusing on the source document, inverted index, and doc values.

### Source Document
- The source document in Elasticsearch refers to the original JSON representation of the indexed data.
- It contains the complete content of the indexed document in its original form, including all fields and values.
- The source document serves as the primary source of truth for the indexed data.

### Inverted Index
- The inverted index is a fundamental data structure used in Elasticsearch for full-text search. It is used to store a mapping of terms to documents that contain them.
- When we index a document, Elasticsearch creates an inverted index for each field that is set up to be indexed.
- It's made for fields that store text and are set up to be indexed. This index breaks down the text into smaller parts called "terms" after processing them (like chopping up words and removing endings). 
- Each term in the index keeps track of which documents contain it, forming what's called a "posting list."
- So, when you search for something, Elasticsearch checks this index to quickly find where the words you're looking for are located in your documents.

### Doc Values
- Doc values are a columnar data storage format optimized for sorting, aggregations, and scripting operations. 
- Doc values indexed are stored 
- Doc values in Elasticsearch are like a well-organized filing system for storing field values.
- When a field is marked for doc values, Elasticsearch stores its values in a way that's perfect for sorting and combining them.
- This format is optimized for tasks like sorting data alphabetically or adding up numerical values across documents.
- So, when you need to perform operations like sorting or aggregating data, doc values make it quick and easy to access the original field values.


### Example
Consider an example of indexing a document representing a product:

```json
{
  "product_id": 123,
  "name": "Elasticsearch Beginner's Guide",
  "category": "Books",
  "price": 29.99,
  "stock_quantity": 100
}
```

- **Source Document**: The original JSON representation of the indexed document.
- **Inverted Index**: Terms extracted from textual fields like "name" and "category", along with their associated posting lists.
- **Doc Values**: Raw field values for "product_id", "price", and "stock_quantity" stored in a columnar format for efficient sorting and aggregation operations.

### Inverted Index Analysis

This analysis demonstrates how terms from sentences in documents are indexed in the inverted index, facilitating efficient search and retrieval operations in Elasticsearch based on document content.

**Sentences from Documents:**
1. Document ID 1: "The quick brown fox"
2. Document ID 2: "Jumped over the lazy dog"
3. Document ID 3: "The brown fox is quick"

**Inverted Index Table:**

| Term   | Document IDs    |
|--------|-----------------|
| The    | 1, 2, 3         |
| quick  | 1, 3            |
| brown  | 1, 3            |
| fox    | 1, 3            |
| jumped | 2               |
| over   | 2               |
| lazy   | 2               |
| dog    | 2               |
| is     | 3               |

**Explanation:**
- Each row in the table represents a term extracted from the sentences in the documents.
- The "Term" column lists individual terms, while the "Document IDs" column shows the document IDs where each term appears.
- Terms like "The," "quick," "brown," and "fox" appear in multiple documents, indicating their presence across different documents.
- Conversely, terms like "jumped," "over," "lazy," "dog," and "is" appear in specific documents, demonstrating their occurrence in individual documents.
---

### Field Analysis in Elasticsearch

This document provides an analysis of different fields and their storage characteristics in Elasticsearch. Each field in the provided JSON example is categorized based on its data structure, data type, and storage type.

### Example JSON:
```json
{ 
  "name": "Akshit",
  "age": 23,
  "height": 6.1,
  "hobbies": ["cricket", "football"],
  "id": [{ "name": "adhar", "value": "123"}, { "name": "pan", "value": "133"}],
  "address": {
    "city": "pune",
    "state": "maharastra"
  },
  "email": "akshit@example.com"
}
```

### Field Analysis:

| Field            | Data Structure | Data Type          | Storage Type |
|------------------|----------------|--------------------|--------------|
| name             | Inverted Index | Text               | Text-Based   |
| age              | Doc Values     | Integer            | Numeric      |
| height           | Doc Values     | Float              | Numeric      |
| hobbies          | Inverted Index | Text Array         | Text-Based   |
| id               | Nested Field   | Object Array       | Structure    |
| id.name          | Inverted Index | Text               | Text-Based   |
| id.value         | Doc Values     | Text               | Text-Based   |
| address          | Nested Field   | Object             | Structure    |
| address.city     | Inverted Index | Text               | Text-Based   |
| address.state    | Inverted Index | Text               | Text-Based   |
|email       | Doc values	    |   Keyword  | Text-Based    |

---

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

## Data Types

Elasticsearch supports various data types, each designed for specific types of data and use cases. Below are the commonly used data types in Elasticsearch:

### Data Types and Use Cases:
Certainly! Here are combined sentences for each data type:

1. **Text**: Used for full-text search and indexing of large blocks of text data, ideal for indexing textual content such as descriptions or articles.

2. **Keyword**: Stores exact values for structured data, suitable for fields that require exact matching and aggregation like IDs or categories.

3. **Numeric Types**:
   - **Integer**: Stores whole numbers without decimal points, commonly used for counting or indexing.
   - **Float**: Stores numbers with decimal points, suitable for storing precise numerical values like prices or measurements.
   - **Long**: Similar to integer but with a larger range, often used for large numerical values.
   - **Double**: Similar to float but with a larger precision, suitable for scientific calculations or financial data.

4. **Date**: Specifically designed for handling date and time values, supporting various formats and time zones, ideal for indexing temporal data such as timestamps or event dates.

5. **Boolean**: Represents boolean values, allowing true/false indexing and filtering, suitable for fields indicating binary states like status or availability.

6. **Binary**: Used for storing binary data such as images or files, enabling the storage and retrieval of non-textual content.

```json
{
  "text_field": "This is a sample text field.",    // Text data type
  "keyword_field": "category1",                    // Keyword data type
  "integer_field": 42,                             // Integer data type
  "float_field": 3.14,                             // Float data type
  "long_field": 9876543210,                        // Long data type
  "double_field": 1234.5678,                       // Double data type
  "date_field": "2022-01-01T00:00:00Z",            // Date data type
  "boolean_field": true,                           // Boolean data type
  "binary_field": "VGhpcyBpcyBhIHNhbXBsZSBiaW5hcnk=" // Binary data type (base64 encoded)
}
```

## Index Template

- Index templates in Elasticsearch are used to automatically apply predefined settings and mappings to new indices that match certain patterns.
- They are particularly useful for ensuring consistency across multiple indices and simplifying index management.

### DSL for Index Template:

```json
PUT /_index_template/template_name
{
  "index_patterns": ["pattern1", "pattern2"],
  "priority": 1,
  "template": {
    "settings": {
      // Index settings
    },
    "mappings": {
      // Index mappings
    },
    "aliases": {
      // Index aliases
    }
  }
}
```

### Components of Index Template DSL:

- **index_patterns**: An array of index patterns to which the template should be applied. Supports wildcard patterns.
- **priority**: (Optional) An integer value representing the priority of the template. Lower values indicate higher priority.
- **template**: The template definition containing settings, mappings, and aliases to be applied to matching indices.

### Example Index Template DSL:

Here's a simple example of an index template DSL:

```json
PUT /_index_template/logs_template
{
  "index_patterns": ["logs-*"],    // Index pattern matching the template
  "template": {
    "settings": {
      "number_of_shards": 3,       // Define the number of primary shards for matching indices
      "number_of_replicas": 1      // Define the number of replica shards for matching indices
    },
    "mappings": {
      "properties": {
        "message": {
          "type": "text"           // Define the mapping for the "message" field as text
        },
        "timestamp": {
          "type": "date"           // Define the mapping for the "timestamp" field as date
        }
      }
    }
  }
}

```
Now if we insert doc to an index logs-1, Elasticsearch automatically applies the settings and mappings defined in the "logs_template" index template to the "logs-1" index, ensuring consistency across indices.
```json
PUT /logs-1/_doc/1
{
  "message": "This is a log message.",
  "timestamp": "2022-04-01T12:00:00"
}

```

## Elastic Common Schema (ECS)
- ECS, or the Elastic Common Schema, is a standardized data model introduced by Elastic to facilitate consistency and interoperability across various Elastic products and solutions.
- It defines a common set of field names and data types for describing log and metric data, making it easier to analyze and visualize data from different sources within the Elastic Stack.
- By adhering to ECS, users can simplify data integration, correlation, and visualization workflows, enabling more efficient and effective data analysis and monitoring.