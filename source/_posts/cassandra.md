---
title: cassandra
date: 2016-12-21 08:43:00
tags: 工具
---

# CQLSH

```
CREATE KEYSPACE sz
 WITH REPLICATION=
 {'class':'SimpleStrategy', 'replication_factor':1};

CREATE KEYSPACE sh
 WITH REPLICATION=
 {'class':'SimpleStrategy', 'replication_factor':1}; 

```

# Type System
text : utf-8
ascii
## Dates and times
dates and times can be stored using timestamp, which holds data/time data at millisecond precision.
## Timeuuid
has specical functionality to convert between uuid and timestamps.
UUID(timestamp+mac, 8-4-4-4-12 literal)
## Blobs
blob store unstructured binary data such as image, audio, and encrypted data.

# Partition key
the actual ordering is determined by the token of the primary key.
```
SELECT "username", token("username")
FROM users;

```

# Mental model 
```
Map<RowKey, SortedMap<ColumnKey, ColumnValue>>
```