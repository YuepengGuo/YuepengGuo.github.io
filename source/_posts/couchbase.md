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
```