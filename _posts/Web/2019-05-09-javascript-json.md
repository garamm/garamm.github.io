---
layout: post
title: "[JavaScript] JavaScript ↔ JSON"
date: "2019-05-09 00:00"
updated: []
categories: [Web, JavaScript]
tags: [json]
---

**JSON → JavaScript**
```javascript
var accountStr = '{"name":"John", "members":["Sam", "Smith"], "number":12345, "location":"Seoul"}';
var accountObj = JSON.parse(accountStr);
console.log(accountObj.name);
console.log(accountObj.members);
```
<br>
**JavaScript → JSON**
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