---
layout: posts
title: "[VueJS] 자바스크립트에서 변수 참조가 안될 때"
comments: true
categories: [VueJS]
---

방법 1. this를 새로운 변수에 저장해서 접근하도록 수정한다.
```javascript
created() {
    var self = this; // this를 self라는 변수에 저장
    axios.get("http://127.0.0.1/api/test")
    .then(function (response) {
        self.data = response.data;
    });
}
```

방법 2. => 를 사용한다.
```javascript
created() {
    axios.get("http://127.0.0.1/api/test")
    .then((response) => {
        this.data = response.data;
    });
}
```