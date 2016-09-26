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






