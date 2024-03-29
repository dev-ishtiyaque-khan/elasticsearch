
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
