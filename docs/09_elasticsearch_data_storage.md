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
