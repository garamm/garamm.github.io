---
layout: posts
title: "[JavaScript] Javascript 배열 중복 제거"
comments: true
categories: [Html/CSS/JavaScript]
---

```javascript
function removeArrayDuplicate(array) {
	var a = {};
	for(var i=0; i <array.length; i++){
		if(typeof a[array[i]] == "undefined")
			a[array[i]] = 1;
	}
	array.length = 0;
	for(var i in a)
		array[array.length] = i;
	return array;
}
```