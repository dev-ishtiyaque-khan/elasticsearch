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

7. **Replication**:
   - Creates duplicate copies of shards (replicas) for fault tolerance and high availability.
   - Provides backup copies of data, reducing the risk of data loss and ensuring resilience against node failures.

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
