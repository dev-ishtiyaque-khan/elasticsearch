# ElasticSearch Query
## `_search`
- `_search` is an endpoint in Elasticsearch that allows you to retrieve data from your Elasticsearch indices.
- It's used to perform searches on one or more indices and execute complex queries against your data.

## Query DSL (Domain Specific Language)
- Query DSL is a powerful feature of Elasticsearch that allows you to construct complex queries to search and filter your data.
- It provides a flexible and expressive way to define various types of searches, including full-text searches, term-based searches, and aggregations.
- In Query DSL, queries are expressed as JSON objects, allowing you to specify various criteria such as match queries, term queries, range queries, and more.
- Query DSL also supports aggregations, which enable you to perform analytics and statistical operations on your data.

```json
// GET /index_name/_search

{
  "query": {
    // Your query goes here
  }
  // Additional parameters and options can be included here
}
```

## Relevence Score
- The relevance score is a numerical value that represents how well a document matches a given query.
- This score is crucial in ranking search results, determining which documents are more relevant to the user's query and should appear higher in the results list.

## Pagination in Elasticsearch Queries
- Pagination in Elasticsearch is achieved using the `size` and `from` parameters in a query.
- These parameters help in controlling the number of documents returned and the starting point of the search results.
- The `_source` parameter can be used to include or exclude specific fields from the results.

**DSL Example**

```json
{
  "query": {
    "match_all": {}
  },
  "size": 10,
  "from": 20,
  "_source": ["title", "description"]
}
```

**Explanation**
- **`size`**: The number of documents to return. In this example, `size: 10` means return 10 documents.
- **`from`**: The starting point of the search results. In this example, `from: 20` means start from the 21st document.
- **`_source`**: Specifies which fields to include or exclude in the response. In this example, only the `title` and `description` fields will be included in the results. If `_source` is omitted, all fields are returned by default.

## Query Types
### Match Query
The `match` query is used to search for text, numerics, dates, and boolean values. It analyzes the text and constructs a query for each term in the text.
- It provides several options in match query like `query`, `operator`, `analyzer`, `fuzziness`, `prefix_length`, `prefix_length`, `max_expansions`, `minimum_should_match`, `boost`, `zero_terms_query`, `cutoff_frequency`, `auto_generate_synonyms_phrase_query`.

**DSL Example:**
```json
// shortcut (default operator: or)
{
  "query": {
    "match": {
      "message": "this is a test"
    }
  }
}

// Actual Query
{
  "query": {
    {
      "match": {
        "message": {
          "query": "this is a test",
          "operator": "and"
        }
      }
    }
  }
}
```

**Explanation:**
- The `match` query analyzes the provided text ("this is a test") and searches for each term in the specified field (`message`). It's commonly used for full-text search.
### Disjunction Max Query
- The `dis_max` (Disjunction Max) query is used to combine multiple queries and select documents that match any of the provided queries.
- It scores the documents based on the highest score from any matching query.
- The `tie_breaker` parameter allows some control over how scores from different matching queries are combined.
- Formula: `Final score=Highest score+0.7×Other score`

**DSL Example**
Let's consider a `dis_max` query with two sub-queries: one matching the `title` field and the other matching the `content` field. We'll use two different `tie_breaker` values (0 and 1) to illustrate the calculation.

**Example with `tie_breaker: 0`**

```json
{
  "query": {
    "dis_max": {
      "queries": [
        { "match": { "title": "Elasticsearch" }},
        { "match": { "content": "Elasticsearch" }}
      ],
      "tie_breaker": 0
    }
  }
}
```

**Example with `tie_breaker: 1`**

```json
{
  "query": {
    "dis_max": {
      "queries": [
        { "match": { "title": "Elasticsearch" }},
        { "match": { "content": "Elasticsearch" }}
      ],
      "tie_breaker": 1
    }
  }
}
```

**Explanation**
1. **With `tie_breaker: 0`:**
  - Only the highest score from any matching sub-query is considered.
  - If `Query 1` has a score of 3 and `Query 2` has a score of 5, the final score for the document will be the highest score, which is 5.
2. With `tie_breaker: 1`:**
  - The scores from all matching sub-queries are summed up.
  - If `Query 1` has a score of 3 and `Query 2` has a score of 5, the final score for the document will be the sum of all scores, which is 3 + 5 = 8.

**Example Calculation**
1. **Document Scores for `tie_breaker: 0`:**
  - Document A: `Query 1` score: 3, `Query 2` score: 5
    - Final score: max(3, 5) = 5
2. **Document Scores for `tie_breaker: 1`:**
  - Document A: `Query 1` score: 3, `Query 2` score: 5
    - Final score: 3 + 5 = 8
### Multi-Match Query
- The `multi_match` query is an extension of the `match` query that allows searching for a given term or terms across multiple fields in a document.
- It combines the results from multiple fields and can use various strategies to determine how these fields should contribute to the overall score.

**Example with `type: best_fields`:**
```json
{
  "query": {
    "multi_match": {
      "query": "Elasticsearch",
      "fields": ["title", "content"],
      "type": "best_fields"
    }
  }
}
```

**Explanation:**
Here’s a breakdown of the components:
- **`query`**: "Elasticsearch" – The term we are searching for.
- **`fields`**: ["title", "content"] – The fields in which we are searching for the term.
- **`type`**: "best_fields" – This is the default type. It searches for the best matching field and returns the highest score from any of the matching fields.

**`type`**:
  - `best_fields`: Finds documents which match any field, but uses the _score from the best field.
  - `most_fields`: Finds documents which match any field and combines the _score from each field.
  - `cross_fields`: Treats fields with the same analyzer as though they were one big field. Looks for each word in any field.
  - `phrase`: Runs a match_phrase on each field and uses the _score from the best field.
  - `phrase_prefix`: Runs a match_phrase_prefix query on each field and uses the _score from the best field.
### IDs Query

**Definition**
The `ids` query allows you to search for documents based on their unique IDs. This query is particularly useful when you know the exact IDs of the documents you are looking for and want to retrieve them directly.

**DSL Example**

```json
{
  "query": {
    "ids": {
      "values": ["1", "2", "3"]
    }
  }
}
```

**How It Works:**
- The `ids` query retrieves documents from the index where the document ID matches any of the specified values.
- This type of query is very efficient and fast because Elasticsearch can directly look up documents by their IDs without needing to search through the content.
- When you run the `ids` query with values ["1", "2", "3"], Elasticsearch will return the first three documents since their IDs match the specified values.
### Term Query
- The term query is used to search for documents that contain an exact term in a specified field.
- Unlike match queries, which analyze the text before searching, the term query does not analyze the input term.
- It is useful for exact matching scenarios, such as keyword searches or matching numeric, boolean, or date values.

**DSL Example**
```json
// shortcut
{
  "query": {
    "term": {
      "email": "example@email.com"
    }
  }
}

{
  "query": {
    "term": {
      "email": {
        "value": "example@email.com"
      }
    }
  }
}
```
### Range Query
- The `range` query is used to search for documents that have fields containing values within a specified range.
- This query is useful for numerical, date, or other comparable fields where you want to find documents that fall within a particular range of values.

**DSL Example:**

```json
{
  "query": {
    "range": {
      "age": {
        "gte": 30,
        "lte": 40
      }
    }
  }
}
```
**Explanation**
- The `range` query is used to search for documents where the `age` field has values between 30 and 40, inclusive. Here’s a breakdown of the components:

- **`age`**: The field to search within.
- **`gte`**: 30 – Greater than or equal to 30.
- **`lte`**: 40 – Less than or equal to 40.

**Range Options:**
- `gt`: Greater than
- `lt`: Less than
- `boost`: Boost value to increase the importance of the query
- `format`: Format for parsing dates
### Bool Query
- The `bool` query is a compound query that allows combining multiple queries using boolean logic.
- It is used to create complex queries by combining must, should, must_not, and filter clauses.
- This query provides great flexibility for constructing advanced search queries.

**DSL Example**
Let's use a `bool` query to combine multiple conditions:

- Must match documents where the `status` field is "active".
- Should match documents where the `age` field is between 30 and 40.
- Must not match documents where the `category` field is "excluded".
- Filter documents where the `created_date` field is after "2020-01-01".

```json
{
  "query": {
    "bool": {
      "must": [
        {
          "term": { "status": "active" }
        }
      ],
      "should": [
        {
          "range": {
            "age": {
              "gte": 30,
              "lte": 40
            }
          }
        }
      ],
      "must_not": [
        {
          "term": { "category": "excluded" }
        }
      ],
      "filter": [
        {
          "range": {
            "created_date": {
              "gt": "2020-01-01"
            }
          }
        }
      ]
    }
  }
}
```
**Explanation**
Here’s a breakdown of the components:
1. `must`: Documents must match the provided queries (kind of and operator). In this case, documents must have the status field equal to "active".
2. `should`: Documents that match the provided queries will score higher (kind of or operator). Here, documents with the age field between 30 and 40 will have a higher score, but it's not mandatory for matching. we only have clause `minimum_should_match` which expects integer (eg. 1) to ensure atleast given `should query` match. 
3. `must_not`: Documents must not match the provided queries. Documents with the category field equal to "excluded" will be excluded.
4. `filter`: Documents must match the provided queries, but these clauses do not contribute to the score. Here, documents must have the created_date field greater than "2020-01-01".

**How It Works:**

- The `must` clause ensures that only documents with the `status` field equal to "active" are considered.
- The `should` clause boosts the score of documents that fall within the age range of 30 to 40, but documents outside this range can still match.
- The `must_not` clause excludes any documents with the `category` field equal to "excluded".
- The `filter` clause ensures that only documents created after "2020-01-01" are included, without affecting the score.
### Exists Query
**Definition**
The `exists` query is used to search for documents that contain a specific field. It is useful for filtering out documents where a particular field is missing or null.

**DSL Example**
```json
{
  "query": {
    "exists": {
      "field": "user"
    }
  }
}
```

**Explanation**
Here’s a breakdown of the components:
- **`field`**: "user" – This specifies the field that must exist in the documents for them to match the query.
- The `exists` query checks if the specified field (`user` in this case) is present in the documents.
- If the field is present and has a non-null value, the document matches the query.
- If the field is missing or contains a null value, the document does not match the query.
### Wildcard Query
**Definition**
- The `wildcard` query is used to search for documents that contain terms matching a specified pattern.
- It allows the use of wildcard characters like `*` (matches zero or more characters) and `?` (matches a single character). This query is useful for matching text fields with varying patterns.

**DSL Example:**

```json
{
  "query": {
    "wildcard": {
      "username": {
        "value": "jo*doe"
      }
    }
  }
}
```

**Explanation**
In this example, the `wildcard` query is used to search for documents where the `username` field matches the pattern "jo*doe". Here’s a breakdown of the components:

- **`username`**: The field to search within.
- **`value`**: "jo*doe" – The pattern to match, where `*` represents zero or more characters.

The `wildcard` query checks the specified field (`username` in this case) for terms that match the given pattern. In this example, any `username` that starts with "jo" and ends with "doe" with any number of characters in between will match the query.
### Prefix query
- The `prefix` query is used to search for documents that contain terms starting with a specified prefix.
- It is useful for autocomplete and search-as-you-type functionalities, where you want to match the beginning of a term.

**DSL Example:**

```json
{
  "query": {
    "prefix": {
      "username": {
        "value": "jo"
      }
    }
  }
}
```

**Explanation**
Here’s a breakdown of the components:
- **`username`**: The field to search within.
- **`value`**: "jo" – The prefix to match.

The `prefix` query checks the specified field (`username` in this case) for terms that start with the given prefix.
In this example, any `username` that begins with "jo" will match the query. This type of query is particularly useful for implementing autocomplete features where users are typing partial terms.
### Match Phrase Prefix Query
**Definition**

- The `match_phrase_prefix` query is used to search for documents that contain phrases starting with a specified prefix.
- It is useful for scenarios where you need to find documents matching the beginning of a phrase, making it suitable for autocomplete and incremental search functionalities.

**DSL Example**
```json
{
  "query": {
    "match_phrase_prefix": {
      "description": {
        "query": "quick brown"
      }
    }
  }
}
```

**Explanation**
In this example, the `match_phrase_prefix` query is used to search for documents where the `description` field contains phrases that start with "quick brown". Here’s a breakdown of the components:

- **`description`**: The field to search within.
- **`query`**: "quick brown" – The beginning of the phrase to match.

The `match_phrase_prefix` query checks the specified field (`description` in this case) for phrases that start with the given prefix. In this example, any `description` that begins with "quick brown" followed by any number of terms will match the query. This query is particularly useful for autocomplete features where users are typing parts of a phrase and expect to see relevant suggestions.
### Match Phrase Query
- The `match_phrase` query is used to search for documents that contain an exact sequence of terms.
- It is useful for finding documents where the specified phrase appears in the exact order provided.

**DSL Example:**

```json
{
  "query": {
    "match_phrase": {
      "description": "quick brown fox"
    }
  }
}
```

**Explanation**
Here’s a breakdown of the components:
- **`description`**: The field to search within.
- **`query`**: "quick brown fox" – The exact phrase to match.

The `match_phrase` query ensures that the terms in the specified field appear in the exact order as the provided phrase. In this example, only documents with the `description` field containing the exact sequence "quick brown fox" will match the query. This query is particularly useful when the order of terms is crucial, such as searching for specific titles, quotes, or fixed expressions.
### Nested Query

**Definition**
The `nested` query is used to search for documents containing nested objects or arrays. It allows executing a query against nested documents within a field, ensuring that matches are correctly scoped to individual nested documents. This is crucial when dealing with nested fields where relationships between fields must be preserved.

**DSL Example (Array of Objects)**

Let's use a `nested` query to search for documents where the nested field `comments` contains an object with `author` equal to "John" and `content` containing "Elasticsearch".

```json
{
  "query": {
    "nested": {
      "path": "comments",
      "query": {
        "bool": {
          "must": [
            { "match": { "comments.author": "John" } },
            { "match": { "comments.content": "Elasticsearch" } }
          ]
        }
      }
    }
  }
}
```

**Explanation**

In this example, the `nested` query is used to search for documents where the `comments` field (a nested field) contains an object that meets the specified criteria. Here’s a breakdown of the components:

- **`path`**: "comments" – This specifies the nested field to search within.
- **`query`**: Defines the query to execute against the nested documents. In this case, it's a `bool` query with two `must` clauses.
  - **`must`**: Specifies the conditions that must be met within each nested document:
    - `comments.author`: "John" – The nested object's `author` field must match "John".
    - `comments.content`: "Elasticsearch" – The nested object's `content` field must contain "Elasticsearch".

The `nested` query ensures that the conditions are applied to each individual nested document, rather than the entire array of nested objects. This means that the `author` and `content` fields must both match within the same nested object for the document to be considered a match.

**Example Document (Array of Objects)**

```json
{
  "title": "Post about Elasticsearch",
  "comments": [
    {
      "author": "John",
      "content": "Elasticsearch is amazing!"
    },
    {
      "author": "Jane",
      "content": "I love using Elasticsearch."
    }
  ]
}
```

**DSL Example (Nested Object Field)**

Let's use a `nested` query to search for documents where the nested field `user` contains an object with `first_name` equal to "John" and `last_name` equal to "Doe".

```json
{
  "query": {
    "nested": {
      "path": "user",
      "query": {
        "bool": {
          "must": [
            { "match": { "user.first_name": "John" } },
            { "match": { "user.last_name": "Doe" } }
          ]
        }
      }
    }
  }
}
```

**Explanation**

In this example, the `nested` query is used to search for documents where the `user` field (a nested field) contains an object that meets the specified criteria. Here’s a breakdown of the components:

- **`path`**: "user" – This specifies the nested field to search within.
- **`query`**: Defines the query to execute against the nested documents. In this case, it's a `bool` query with two `must` clauses.
  - **`must`**: Specifies the conditions that must be met within each nested document:
    - `user.first_name`: "John" – The nested object's `first_name` field must match "John".
    - `user.last_name`: "Doe" – The nested object's `last_name` field must match "Doe".

The `nested` query ensures that the conditions are applied to each individual nested document, rather than the entire nested object. This means that the `first_name` and `last_name` fields must both match within the same nested object for the document to be considered a match.

**Example Document (Nested Object Field)**
```json
{
  "post": "A day in the life of a software engineer",
  "user": {
    "first_name": "John",
    "last_name": "Doe",
    "age": 30
  }
}
```

