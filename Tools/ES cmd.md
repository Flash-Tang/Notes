```shell
# Create index with mapping
curl -X PUT http://ipOrDomain:9200/sft_debugging_index_liveish/ -H 'Content-Type: application/json' -d '{
"mappings": {
    "properties": {
        "model_input_vector": {
            "type": "dense_vector",
            "dims": 384,
            "index": true,
            "similarity": "cosine"
        },
        "model_input": {
            "type": "text"
        }
    }
}}'

# Count number of docs
curl -X GET "domain:9200/traffic_item_id/_count?pretty"

# Get single doc
curl -X GET "domain/traffic_item_v2_liveish_id/_doc/532:6677355200?pretty"

# Search
curl -X GET "domain:9200/traffic_item_id/_search?pretty"

# Delete all documents from data stream or index:
curl -X POST "domain:9200/traffic_item_v2_liveish_id/_delete_by_query?pretty" -H 'Content-Type: application/json' -d' { "query": { "match_all": {} } } '

# Get index mapping
curl -X GET "domain:9200/traffic_item_id/_mapping?pretty"

# alias -> index
curl -X GET "domain:9200/_alias/traffic_item_br?pretty"

# index -> alias
curl -X GET "domain:9200/traffic_item_20230329_th/_alias?pretty"

# Switch alias in a single atomic operation
curl -X POST "domain:9200/_aliases?pretty" -H 'Content-Type: application/json' -d'{"actions": [{"remove": {"index": "traffic_item_20230329_id","alias": "traffic_item_id"}}, {"add": { "index": "traffic_item_20230529_id", "alias": "traffic_item_id" }}]}'

# Check if index exits
curl -I "domain:9200/my-index?pretty"
```



## Seach API

+ match query

  + Returns documents that match a provided text, number, date or boolean value. The provided text is analyzed before matching.

    The `match` query is the standard query for performing a full-text search, including options for fuzzy matching.

  + ```shell
    GET /_search
    {
      "query": {
        "match": {
          "message": {
            "query": "this is a test"
          }
        }
      }
    }
    ```

  + 
