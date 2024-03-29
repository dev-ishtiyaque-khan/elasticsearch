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
