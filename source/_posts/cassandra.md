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
# Clustering key
determine the ordering of rows within a partition.

# Mental model 
```
Map<RowKey, SortedMap<ColumnKey, ColumnValue>>
```
# Static Column
```
CREATE TABLE "users_with_status_updates" (
  "username" text,
  "id" timeuuid,
  "email" text STATIC,
  "encrypted_password" blob STATIC,
  "body" text,
  PRIMARY KEY ("username", "id")
);


Since the email and encrypted_password columns are properties of a user, 
not of a specific status update, we declare them STATIC. 
Any column that is declared STATIC has one value per partition key. 
```
# TIMEUUID
```
CREATE TABLE "user_status_updates" (
  "username" text,
  "id" timeuuid,
  "body" text,
  PRIMARY KEY ("username", "id")
);

CREATE TABLE "reversed_user_status_updates" (
  "username" text,
  "id" timeuuid,
  "body" text,
  PRIMARY KEY ("username", "id")
) WITH CLUSTERING ORDER BY ("id" DESC);

SELECT "username", "id", "body", DATEOF("id")
FROM "user_status_updates";


SELECT "username", "id", "body", UNIXTIMESTAMPOF("id")
FROM "user_status_updates";

INSERT INTO "user_status_updates" ("username", "id", "body")
VALUES ('bob', NOW(), 'Bob Update 3');
```
## Query
```
SELECT "id", DATEOF("id"), "body"
FROM "user_status_updates"
WHERE "username" = 'alice' //partition key
AND "id" >= MINTIMEUUID('2014-05-01') //clustering key
AND "id" <= MAXTIMEUUID('2014-05-31');


SELECT "observed_at", "client_type"
FROM "status_update_views"
WHERE "status_update_username" = 'alice'
  AND "status_update_id" = 76e7a4d0-e796-11e3-90ce-5f98e903bf02
  AND "observed_at" >= MINTIMEUUID('2014-10-05 00:00:00+0000')
  AND "observed_at"  < MINTIMEUUID('2014-10-06 00:00:00+0000');

```

## Aggregation Counter
```
CREATE TABLE "daily_status_update_views" (
  "year" int,
  "date" timestamp,
  "total_views" counter,
  "web_views" counter,
  "mobile_views" counter,
  "api_views" counter,
  PRIMARY KEY (("year"), "date")
);

INSERT INTO "status_update_views" (
  "status_update_username", "status_update_id",
  "observed_at", "client_type"
) VALUES (
  'alice', 76e7a4d0-e796-11e3-90ce-5f98e903bf02,
  85a53d10-4cc3-11e4-a7ff-5f98e903bf02,
  'web'
);

UPDATE "daily_status_update_views"
SET "total_views" = "total_views" + 1,
    "web_views" = "web_views" + 1
WHERE "year" = 2014
  AND "date" = '2014-10-05';
```

