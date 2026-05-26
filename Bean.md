# 핵심 요약
- POJO : 단순 Java 객체
- Bean : Spring container(ApplicationContext)에서 생성, 등록되고 관리되는 객체
- 주 책임 모듈은 spring-beans
- 관련 모듈은 spring-context

예를 들어,
``` java
@Service
public class OrderService {
}
```

이 클래스 자체가 Bean인 것이 아니라, 더 정확히는 이 클래스로부터 만들어져 Spring Container에 등록되고 관리되는 객체 인스턴스가 Bean이다.

| 구분        | POJO     | Bean                  |
| --------- | -------- | --------------------- |
| 생성 주체     | 개발자 코드   | Spring Container      |
| 의존성 주입 주체 | 개발자 코드   | Spring Container      |
| 생명주기 관리   | 직접 관리    | Spring이 관리            |
| 싱글톤 관리    | 직접 구현 필요 | 기본적으로 Singleton scope |
| AOP 적용?   | 직접 불가능   | Spring을 통해 가능         |
