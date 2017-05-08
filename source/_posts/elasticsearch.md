---
title: elasticsearch
date: 2017-05-04 08:50:18
tags: 工具
---

## CRUD
```bash
curl -XPUT "http://localhost:9200/movies/movie/1" -d'
{
    "title": "The Godfather",
    "director": "Francis Ford Coppola",
    "year": 1972
}'

curl -XGET "http://localhost:9200/movies/movie/1" -d''

curl -XDELETE "http://localhost:9200/movies/movie/1" -d''

curl ...:9200/names/name -XPOST -d '{
        "userId": 10,
        "name": {
            "first": "Katherine",
            "last": "Jones"
        },
        "tags": ["trick manager", "restaurant manager"]
    }'

 ##create index

 curl -XPOST localhost:9200/myindex -d '{
        "settings" : {
            "number_of_shards" : 2,
            "number_of_replicas" : 1
        },
        "mappings" : {
          "order" : {
              "properties" : {
                  "id" : {"type" : "keyword", "store" : "yes"},
                  "date" : {"type" : "date", "store" : "no" , 
                  "index":"not_analyzed"},
                  "customer_id" : {"type" : "keyword", "store" : 
                                  "yes"},
                  "sent" : {"type" : "boolea+n", 
                            "index":"not_analyzed"},
                  "name" : {"type" : "text", "index":"analyzed"},
                  "quantity" : {"type" : "integer", 
                                "index":"not_analyzed"},
                  "vat" : {"type" : "double", "index":"no"}
              }
          }
      }
    }'

    ##delete index
    curl -XDELETE http://127.0.0.1:9200/myindex

##put mappings in index
curl -XPUT 'http://localhost:9200/myindex/order/_mapping' -d '{
        "order" : {
        "properties" : {
        "id" : {"type" : "keyword", "store" : "yes"},
        "date" : {"type" : "date", "store" : "no" , 
                  "index":"not_analyzed"},
        "customer_id" : {"type" : "keyword", "store" : "yes"},
        "sent" : {"type" : "boolean", "index":"not_analyzed"},
        "name" : {"type" : "text", "index":"analyzed"},
        "quantity" : {"type" : "integer", 
                      "index":"not_analyzed"},
        "vat" : {"type" : "double", "index":"no"}
       }
    }
}'

##get mappings
curl -XGET 'http://localhost:9200/myindex/order/_mapping?pretty=true'

##get document
curl -XGET  'http://localhost:9200/myindex/order/2qLrAfPVQvCRMe7Ku8r0Tw?pretty=true'
curl 'http://localhost:9200/myindex/order/2qLrAfPVQvCRMe7Ku8r0Tw?fields=date,sent'
curl -XGET http://localhost:9200/myindex/order/2qLrAfPVQvCRMe7Ku8r0Tw/_source

##delete document
curl -XDELETE  'http://localhost:9200/myindex/order/2qLrAfPVQvCRMe7Ku8r0Tw'

##update document
http://<server>/<index_name>/<type_name>/<id>/_update


```

## Searching

+ http://localhost:9200/_search - Search across all indexes and all types.
+ http://localhost:9200/movies/_search - Search across all types in the movies index.
+ http://localhost:9200/movies/movie/_search - Search explicitly for documents of type movie within the movies index.

```bash
curl -XPOST "http://localhost:9200/_search" -d'
{
    "query": {
        "query_string": {
            "query": "kill"
        }
    }
}'

curl -XPOST "http://localhost:9200/_search" -d'
{
    "query": {
        "query_string": {
            "query": "ford",
            "fields": ["title"]
        }
    }
}'

#filter
curl -XPOST "http://localhost:9200/_search" -d'
{
    "query": {
        "filtered": {
            "query": {
                "query_string": {
                    "query": "drama"
                }
            },
            "filter": {
                "term": { "year": 1962 }
            }
        }
    }
}'

curl -XPOST "http://localhost:9200/_search" -d'
{
    "query": {
        "filtered": {
            "query": {
                "match_all": {
                }
            },
            "filter": {
                "term": { "year": 1962 }
            }
        }
    }
}'

curl -XPOST "http://localhost:9200/_search" -d'
{
    "query": {
        "constant_score": {
            "filter": {
                "term": { "year": 1962 }
            }
        }
    }
}'

#mapping
curl -XPOST ...:9200/my_index -d '{
    "settings" : {
        # .. index settings
    },
    "mappings" : {
        "my_type" : {
            # mapping for my_type
        }
    }
}'


```