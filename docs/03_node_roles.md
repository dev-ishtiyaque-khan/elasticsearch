
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
