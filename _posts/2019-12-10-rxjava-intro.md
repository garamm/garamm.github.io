---
layout: post
title:  "RXJava Intro"
date:   2019-12-10 00:14:27 +0900
categories: rxjava
---

### Netflix에서 RXJava를 만들게 된 이유
-----
<br>

#### 1. 동시성 수용(Embrace Concurrency)
네트워크 통신을 효과적으로 줄이기 위해서는 서버측 동시성이 필요하다.<br>
RXJava는 클라이언트의 요청을 처리할 때 **다수의 비동기 스레드를 생성하고 그 결과를 취합하여 최종 리턴**하는 방식으로 진행된다.
<br>
<br>

#### 2. Java Future를 조합하기 어렵다(Java Futures are Expensive to Compose)
> Java Future<br><br>
실행 결과를 얻기까지 시간이 걸리는 메소드가 있다고 했을 때 실행 결과를 얻기까지 기다리는 대신 "교환권"을 받게 되는데 그 교환권을 Future라고 한다.<br><br>
Future를 받은 쓰레드는 나중에 Future를 사용해서 실행 결과를 받으러 간다.<br><br>
만약 실행 결과가 나와 있으면 바로 그것을 받고, 그렇지 않으면 준비가 될 때 까지 기다린다.

Java 8 에서 CompletableFuture 같은 클래스를 제공하지 않았기 때문에 **비동기 흐름을 조합할 수 있는 방법**이 필요해서 제작했다.
<br>
<br>

#### 3. 콜백 방식의 문제점 개선(Callbacks Have Their Own Problems)
콜백(Callback; 시스템이 필요한 시점에 호출하는 이벤트)안에서 또 다른 콜백을 부르는 코딩 방식은 코드의 가독성을 떨어뜨리고 오류 발생시 디버깅이 어렵다는 단점이 있다. 이를 개선하기 위해 RXJava를 제작했다.
<br>
<br>

### RXJava를 시작하기 전에 알아야 할 것들
-----
<br>

#### 1. Observer Pattern
<br>
<br>

#### 2. 람다 표현식
<br>
<br>

#### 3. Method Reference
<br>
<br>




출처:<br>
&nbsp;&nbsp;[Netflix 기술블로그]


[Netflix 기술블로그]: https://medium.com/netflix-techblog/reactive-programming-in-the-netflix-api-with-rxjava-7811c3a1496a


<!-- ~~Oh My God~~
`_posts` -->