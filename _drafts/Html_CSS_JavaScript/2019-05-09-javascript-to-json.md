---
layout: post
title: "[JavaScript] Javascript <-> JSON"
categories: JavaScript
---

1. json -> java script
```javascript
var accountStr = '{"name":"John", "members":["Sam", "Smith"], "number":12345, "location":"Seoul"}';
var accountObj = JSON.parse(accountStr);
console.log(accountObj.name);
console.log(accountObj.members);
```
   
2. java script -> json
```javascript
var accountObj = {
	"name":"John",
	"members":["Sam", "Smith"],
	"number":12345,
	"location":"Seoul"
}
var accountStr = JSON.stringify(accountObj);
console.log(accountStr);
```