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
