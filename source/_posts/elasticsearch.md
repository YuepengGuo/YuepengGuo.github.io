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


```