## Analyzer
- In Elasticsearch, an analyzer is a configurable component responsible for preprocessing and transforming text during indexing and search operations. It consists of a tokenizer and optional token filters, allowing users to customize how text is broken down into tokens and how those tokens are modified. Analyzers are crucial for tasks like stemming, stop words removal, lowercase conversion, and more, ensuring efficient and accurate search results.

### Types of Analyzers:
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