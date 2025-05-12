In Elasticsearch, **analyzers** are components that process text during **indexing** and **searching** to make it searchable. An analyzer breaks text into terms (tokens) and optionally transforms them—for example, by making them lowercase or removing stop words like “a,” “the,” etc.

---

### 🔧 An Analyzer consists of three main parts:

1. **Character filters** (optional): Preprocess the text (e.g., remove HTML tags).
2. **Tokenizer** (required): Splits the text into individual terms (tokens).
3. **Token filters** (optional): Modify each token (e.g., lowercase, stemming, synonyms).

---

### 🧪 Example

If you index the string:

```
"Running faster than the wind"
```

With the default analyzer, it might get processed into tokens like:

```
["running", "faster", "than", "the", "wind"]
```

And if you add a lowercase filter and a stemmer:

```
["run", "faster", "than", "the", "wind"]
```

---

### 📦 Built-in Analyzers in Elasticsearch:

* `standard` (default): Uses standard tokenizer and filters.
* `simple`: Splits text on non-letter characters.
* `whitespace`: Splits on spaces only.
* `stop`: Filters out common stop words.
* `keyword`: Treats the whole text as a single token.
* `pattern`: Uses regex for splitting.

---

### 🧰 Custom Analyzer (for E-commerce example):

For searching products like `T-shirt`, `tshirt`, or `TShirt`, a custom analyzer can normalize these using filters.

```json
"settings": {
  "analysis": {
    "analyzer": {
      "product_name_analyzer": {
        "type": "custom",
        "tokenizer": "standard",
        "filter": ["lowercase", "asciifolding", "custom_word_filter"]
      }
    },
    "filter": {
      "custom_word_filter": {
        "type": "synonym",
        "synonyms": [
          "t-shirt, tshirt => tshirt"
        ]
      }
    }
  }
}
```

Here's a complete example of how to create an Elasticsearch index with a **custom analyzer** for handling product names like `T-shirt`, `tshirt`, and `TShirt`. This helps normalize synonyms and improve search accuracy.

---

### ✅ Request: Create index with custom analyzer

```bash
PUT /products
{
  "settings": {
    "analysis": {
      "analyzer": {
        "product_name_analyzer": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "asciifolding",
            "custom_synonyms"
          ]
        }
      },
      "filter": {
        "custom_synonyms": {
          "type": "synonym",
          "synonyms": [
            "t-shirt, tshirt, tee shirt => tshirt",
            "jeans, denim => jeans",
            "mobile phone, smartphone => smartphone"
          ]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "product_name": {
        "type": "text",
        "analyzer": "product_name_analyzer"
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

---

### ✅ Example Document to Index

```bash
POST /products/_doc
{
  "product_name": "T-Shirt for men",
  "price": 499.99,
  "category": "clothing"
}
```

---

### 🔍 Example Search Query

```bash
GET /products/_search
{
  "query": {
    "match": {
      "product_name": "tshirt"
    }
  }
}
```

This will return the document even if it was originally stored as `T-Shirt`.
