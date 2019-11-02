---
title: "[Android/Java] 이메일 형식이 맞는지 검증하기"
categories:
  - Android/Java
---

```java
if(Patterns.EMAIL_ADDRESS.matcher(inputText).matches()) {
    // 유효한 이메일
} else {
    // 유효하지 않은 이메일
}
```