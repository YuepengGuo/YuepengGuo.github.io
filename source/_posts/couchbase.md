---
title: couchbase
date: 2016-10-25 10:26:12
tags: 工具
---

## Indexing and Querying via Views
```
#primary index - all documents by id
function map (doc,meta){
	emit(meta.id,null)
}
#secondary index by name
function map (doc,meta){
	if(typeof doc.name !== undefined){
	//emiting null means you later access the id of the row
	emit(doc.name,null);
}
}

#compound key -- create a view on persons by indexing by the birthday
function map(doc,meta){
	if(doc.birthday){
	emit(dateToArray(doc.birthday),{"firstname":doc.firstname,"lastname":doc.lastname});
}
}

//access
.../_view/byname?reduce=false&key="David"
.../_view/byname?reduce=false&startkey="P"&endKEY="S"
.../_view/byname?reduce=true&startkey="P"&endKEY="S"
.../_view/bybirthday?reduce=true&group_level=1

```

## Views
 * Organized in Design Documents
 * Incremental Map-Reduce

## Best Practices
 * Avoid computing too many things in a view
 * Check for attribute existence
 * Pre-Filter to avoid unnecessary entries in the view (if(doc.type=="person")...)
 * Project(map) only necessary data by emiting it as part of the value
 * Use the built-in reduce if possible

