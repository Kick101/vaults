## Searching

#### 🔍 `query` vs `filter`

|Feature|`query`|`filter`|
|---|---|---|
|**Used for**|Full-text relevance search|Exact matching / boolean logic|
|**Scoring**|Yes – affects `_score`|No – filter results are not scored|
|**Performance**|Slower (uses scoring algorithms)|Faster (can be cached and skipped easily)|
|**Use case**|Search for "best match" (relevance)|Narrow results down with exact criteria|


### 🧠  `bool`

A `bool` query lets you **combine multiple queries** using logical operators:

|Clause|Purpose|Scoring?|
|---|---|---|
|`must`|All queries must match (AND)|Yes|
|`filter`|All filters must match (AND)|No|
|`should`|Optional, boosts score if matches (OR)|Yes (unless required)|
|`must_not`|Must **not** match (NOT)|No|

>#### 🔥 Tips
>- If you use **only** `should`, at least one should match.
>- If you mix `must`/`filter` with `should`, the `should` clauses just boost scoring.
>- Use `minimum_should_match` to control how many `should` clauses must match.


```json
GET /my-index/_search
{
  "query": {
    "match": {
      "description": "fast red car"
    }
  }
}
```

```json
GET /my-index/_search
{
  "query": {
    "bool": {
      "must": {
        "match": { "description": "fast red car" }
      },
      "filter": {
        "term": { "status": "active" }
      }
    }
  }
}
```

**Multi match**  — Search **across multiple fields** with a single input.

```json
"query" : {
	"multi_match" : {
		"query" : "John",
		"fields" : ["l_name", "f_name"]
	}
}
```

**Exists** — Check if a filed exists in docs

```json
"query" : {
	"exists" : {"field" : "src_ip"}
}
```

**Range** — get docs with sal b/w 1000 - 2000 inclusive.

```json
"query" : {
	"range" : {
		"sal" : {
			"gte" : 1000,
			"lte" : 2000
		}
	}
}
```

> #### ✅ Rule of Thumb
>- Use **`query`** when you're doing **full-text search** and want to **rank results**.
>- Use **`filter`** for **exact matches**, **range filters**, **dates**, **booleans**, and anything where **scoring doesn't matter**.





