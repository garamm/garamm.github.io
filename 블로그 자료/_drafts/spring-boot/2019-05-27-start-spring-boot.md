---
layout: post
title: "SpringBoot 정리중"
comments: true
category: spring-boot
date: "2019-05-27"
---

---
IoC(Inversion of Control) : 제어권 역전
   
일반코드 : 내가 사용할 의존성을 내가 만든다.
```java
class OwnerController {
    private OwnerRepository repository = new OwnerRepository();
}
```
IoC : 내가 사용할 의존성을 받아온다. 의존성 타입(혹은 인터페이스)만 같으면 상관 없다. 객체를 내부에서 만들지 않고 밖에서 선언된 객체를 생성자를 통해 받아옴 -> 의존성을 만드는 일을 OwnerController가 하는것이 아니라 그 밖에서 생성 -> 제어권의 역전!
```java
class OwnerController {
    private OwnerRepository repo;

    public OwnerController(OwnerRepository repo) {
        this.repo = repo;
    }
}
```

---
IoC 컨테이너   

ApplicationContext(Bean Factory) : 빈(bean)을 만들고 엮어주며 제공해준다.   
ApplicationContext 안에 모든 bean들이 들어있고, bean으로 등록되어 있는 객체들끼리만 의존성 주입이 가능하다.   

---
Bean : IoC 컨테이너가 관리하는 객체   
```java
OwnerController ownerController = new OwnerController(); // bean이 아님
OwnerController bean = applicationContext.getBean(OwnerController.class); // bean, 그러나 이렇게 쓰이지는 않음, @Autowired로 많이 쓰임
```
bean = application context가 알고있는 객체를 의미   

bean 등록하는 방법  
(1) Component Scan  
@Component를 찾아서 그 클래스의 인스턴스를 만들어서 bean으로 등록해주는 어노테이션  
@ComponentScan이 붙은 package 밑의 모든 클래스를 확인하면서 @Component를 찾아서 bean으로 등록해준다.  
SpringBoot에서는 @SpringBootApplication 안에 포함되어 있음  
@Component  
 ㄴ @Repository  
 ㄴ @Service  
 ㄴ @Controller  
  
(2) @Bean으로 등록  
  
bean 사용하는 방법  
Autowired  
사용방법 : @Autowired 또는 @Inject 또는 applicationContext.getBean()으로 직접 꺼내기

---
DI(Dependency Injection) : 의존성 주입
@Autowired / @Inject 붙이는 곳
(1) 생성자 // Spring에서 권장하는 방법
(2) 필드
(3) Setter
   
---
AOP : 흩어진 코드를 한곳으로 모음   
   
같은 기능을 A class, B class에서 사용한다면, 같은 기능을 다른 클래스로 묶어서 사용한다.  
 ㄴ 나중에 그 기능을 변경하려면 A class, B class 모두 바꿔줘야 하기 때문에 따로 빼서 관리하면 훨신 편함  
 ㄴ @Transactional : https://goddaehee.tistory.com/167  
 ㄴ ex. 모든 실행되는 함수의 수행시간을 얻기 위해 StopWatch를 사용해야 하는데 모든 함수 내에 StopWatch를 선언하기엔 너무 비효율적임 -> StopWatch를 AOP를 사용하여 함수 내에 선언하지 않고도 모든 함수에 해당 기능을 사용 가능  
  
AOP 구현방법  
(1) 컴파일  
  A.java -> (AOP) -> A.class(AspectJ)  
(2) 바이트코드 조작  
  A.java -> A.class -> (AOP) -> 메모리 : Class Loader가 class를 읽어서 메모리를 올릴 때 AOP 삽입  
(3) 프록시 패턴  
   

실제 사용법  
예제. 해당 메소드를 실행할 때 소요되는 시간을 확인하는 AOP를 프록시패턴 기반으로 작성  
```java
// LogExecutionTime.java
@Target(ElementType.METHOD) // 어디에 쓸 수 있는지 타겟 설정
@Retention(RetentionPolicy.RUNTIME) // 어노테이션 정보를 언제까지 유지할지 설정
public @interface LogExecutionTime {

}

// LogAspect.java
@Compoenent
@Aspect
public class LogAspect {

    Logger logger = LoggerFactory.getLogger(LogAspect.class);

    @Around("@anotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        // joinPoint = @LogExecutionTime이 붙어있는 메소드(LogExecutionTime의 target을 메소드로 지정했기 때문)를 의미함.
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        Object proceed = joinPoint.proceed();

        stop.Watch.stop();
        logger.info(stopWatch.prettyPrint());

        return proceed;
    }
}


// Controller.java 실제사용하기
@LogExecutionTime
public String funcA() {

}
```
---
PSA(Portable Service Abstraction) : 잘 만든 인터페이스 
 ㄴ 환경의 변화와 관계 없이 일관된 방식의 기술로의 접근 환경을 제공하려는 추상화 구조  
  
(1) 스프링 MVC  
@RequestMapping  
  
(2) 스프링 트랜잭션  
@Transactional  
  
(3) 스프링 캐시  
@Cacheable  

