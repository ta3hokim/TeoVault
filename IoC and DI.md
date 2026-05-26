# 핵심 요약
- IoC는 객체 생성과 의존 관계 제어가 개발자 코드에서 컨테이너로 넘어가는 구조이고
- DI는 그 결과로 객체의 의존 관계를 외부에서 넣어주는 방식이다.
- 주 책임 모듈은 spring-beans
- 관련 모듈은 spring-core, spring-context

# IoC
Inversion of Control, "제어의 역전"

일반 Java에서는 `new`를 사용해서 직접 객체를 만든다.
이 경우 제어권은 개발자 코드에 있다.

그런데, Spring에서는 `new`를 사용해서 직접 객체를 만들지 않는다.
Spring Container가 객체를 만들고, 필요한 의존 객체도 주입한다.

즉, 일반 Java에서는 "개발자 코드"가 객체 생성과 연결을 제어하지만 Spring에서는 "Container"가 객체 생성과 연결을 제어한다.

이것을 Inversion of Control이라고 한다.

# DI
Dependency Injection, "의존성 주입"

```java
@Service
public class OrderService {
    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

`OrderService`는 `OrderRepository`가 필요하다.
이 때 `OrderService` 안에서 직접 객체를 만들지 않고 "외부에서 넣어준다"
``` java
public OrderService(OrderRepository orderRepository) {
    this.orderRepository = orderRepository;
}
```