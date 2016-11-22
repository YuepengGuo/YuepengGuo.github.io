---
title: neo4j
date: 2016-09-13 12:41:18
tags: 工具
---

## Use cases

#### Real Time Recommendations

+ Dyanmic systems :  where the data topology is difficult to predict vs relational database data are well structured.
+ Dynamic requirements : the evolve with the business
+ Problems where the relationships in data contribute meaning and value.

#The property graph data model

entity becomes node.
relationship are directional.

#### why graph
+ intuitiveness : index free adjacency
+ speed : traversal
+ agility : cypher


cypher is all about pattern matching.

```
MATCH (:Person {name: "Ann"}) - [:FB_FRIENDS] -> (:Person {name:"Dan"})
```


#### Concept
Nodes
Relationships
Properties

Nodes can be grouped together by applying a label to each memeber.
 like Color "Person" nodes red
A Node can have zero or more labels.
labels do not have any properties.

The real power of Neo4j is connected data.

Relationship always have direction.
Relationship always have a type.
Relationship forms patterns of data.

Relationship can contains properties.


Cypher -- Neo4j's graph query language

```
#create
CREATE (ee:Person { name: "Emil", from: "Sweden", klout: 99 })
MATCH (ee:Person) WHERE ee.name = "Emil" RETURN ee;
MATCH (ee:Person) WHERE ee.name = "Emil"
CREATE (js:Person { name: "Johan", from: "Sweden", learn: "surfing" }),
(ir:Person { name: "Ian", from: "England", title: "author" }),
(rvb:Person { name: "Rik", from: "Belgium", pet: "Orval" }),
(ally:Person { name: "Allison", from: "California", hobby: "surfing" }),
(ee)-[:KNOWS {since: 2001}]->(js),(ee)-[:KNOWS {rating: 5}]->(ir),
(js)-[:KNOWS]->(ir),(js)-[:KNOWS]->(rvb),
(ir)-[:KNOWS]->(js),(ir)-[:KNOWS]->(ally),
(rvb)-[:KNOWS]->(ally)

#pattern matching

MATCH (ee:Person)-[:KNOWS]-(friends)
WHERE ee.name = "Emil" RETURN ee, friends

#pattern matching for recommendations
#Johan is learning to surf, so he may want to find a new friend who already does:

MATCH (js:Person)-[:KNOWS]-()-[:KNOWS]-(surfer)
WHERE js.name = "Johan" AND surfer.hobby = "surfing"
RETURN DISTINCT surfer

#Movie

```
