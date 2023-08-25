---
layout: post
title: "[Vue.js] Vue.js CDN으로 시작하기"
date: "2023-03-03 22:52"
updated: []
categories: [Web, Vue]
tags: [cdn]
---

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!-- Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- Polyfill 지원하지 않는 이전 브라우저에서 최신 코드를 지원 가능하도록 변환 -->
    <script src=https://cdn.jsdelivr.net/npm/promise-polyfill@8.1/dist/polyfill.min.js></script>

    <title>Vue.js CDN으로 시작하기</title>
</head>

<body>
    <div id="app">
        {{ num }}
    </div>
</body>

</html>

<script>
    new Vue({
        el: '#app',
        data: {
            num: 3
        },
        created: function () {
            // ...
        },
    });
</script>
```