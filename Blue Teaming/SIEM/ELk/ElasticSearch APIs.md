## 1. **Index APIs**

- **Create an index**
    
```http
PUT /my-index
```
    
- **Delete an index**

```http
DELETE /my-index
```
    
- **Get index settings and mappings**

```http
GET /my-index
```
    
- **Update index settings**
    
```http
PUT /my-index/_settings
{
  "index": {
	"number_of_replicas": 2
  }
}
```

---

## 2. **Document APIs**

- **Index (add/update) a document**
    
```http
POST /my-index/_doc/123
{
  "field": "value"
}
```
    
- **Get a document**
    
```http
GET /my-index/_doc/123
```
    
- **Delete a document**

```http
DELETE /my-index/_doc/123
```
    
- **Update a document**
    
```http
POST /my-index/_update/123
{
  "doc": {
	"field": "new_value"
  }
}
```
    
- **Bulk operations**
    
```http
POST /_bulk
{ "index": { "_index": "my-index", "_id": "1" } }
{ "field": "value1" }
{ "delete": { "_index": "my-index", "_id": "2" } }
```
    

---

## 3. **Search APIs**

- **Basic search**
    
```http
GET /my-index/_search
{
  "query": {
	"match": { "field": "value" }
  }
}
```
    
- **Search with aggregations**
    
```http
GET /my-index/_search
{
  "aggs": {
	"avg_price": { "avg": { "field": "price" } }
  }
}
```
    
- **Scroll API** (for deep pagination)
    
```http
POST /my-index/_search?scroll=1m
{
  "size": 100,
  "query": { "match_all": {} }
}
```
    

---

## 4. **Cluster APIs**

- **Get cluster health**
    
```http
GET /_cluster/health
```
    
- **Get cluster stats**
    
```http
GET /_cluster/stats
```
    
- **Get nodes info**
    
```http
GET /_nodes
```
    
- **Reroute shards**

```http
POST /_cluster/reroute
{
  "commands": [...]
}
```
    

---

## 5. **Snapshot and Restore APIs**

- **Create snapshot repository**
    
```http
PUT /_snapshot/my_backup
{
  "type": "fs",
  "settings": {
	"location": "/mount/backups/my_backup"
  }
}
```
    
- **Take a snapshot**

```http
PUT /_snapshot/my_backup/snapshot_1
```
    
- **Restore a snapshot**
    
```http
POST /_snapshot/my_backup/snapshot_1/_restore
```
    

---

## 6. **Ingest APIs**

- **Put ingest pipeline**
    
```http
PUT /_ingest/pipeline/my_pipeline
{
  "processors": [
	{ "set": { "field": "field1", "value": "value1" } }
  ]
}
```
    
- **Simulate pipeline**
    
```http
POST /_ingest/pipeline/_simulate
{
  "pipeline": { ... },
  "docs": [ { "_source": { ... } } ]
}
```
    

---

## 7. **Security APIs** (if enabled)

- **Create user**
    
```http
POST /_security/user/john
{
  "password": "changeme",
  "roles": [ "admin" ]
}
```
    
- **Get user info**
    
```http
GET /_security/user/john
```
    

---

# Summary

|API Category|Purpose|
|---|---|
|Index APIs|Manage indices|
|Document APIs|CRUD operations on documents|
|Search APIs|Query documents and aggregate data|
|Cluster APIs|Cluster state and management|
|Snapshot APIs|Backup and restore data|
|Ingest APIs|Preprocessing data pipelines|
|Security APIs|Manage users, roles, and permissions|

