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
POST /index_name/_bulk
{ "index" : { "_id" : "1" } }
{ "field1" : "value1" }
{ "delete" : { "_id" : "2" } }
{ "update" : {"_id" : "3"} }
{ "doc" : {"field1" : "updated_value"} }
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
