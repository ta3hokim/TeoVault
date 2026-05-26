# 전체 그림

Core Container
 ├── spring-core : Spring의 가장 low-level의 기반 도구
 ├── spring-beans : Bean 생성, 등록, 의존성 주입
 ├── spring-context : ApplicationContext, 애노테이션, 이벤트, 리소스
 └── spring-expression : SpEL, 동적 표현식 처리

## spring-core

### 역할
spring-core는 Spring 전체의 가장 Low-level의 기반으로 "Spring이 내부적으로 공통으로 사용하는 기본 기능들이 들어 있다"
즉, Bean 관리나 Context 기능이 동작하기 위해 필요한 기초 도구 상자에 가깝다.

### 대표적으로 연결되는 개념
- Resource
- ClassPathResource
- FileSystemResource
- ReflectionUtils
- Assert
- ClassUtils

예를 들어 Spring은 설정 파일이나 클래스패스 리소스를 읽어야 한다.
``` java
new ClassPathResource("application.properties")
```
이런 식의 리소스 추상화가 spring-core의 개념이다.


## spring-beans

### 역할
IoC와 Dependency Injection의 기본 부분을 제공하는 Spring Container의 핵심 엔진이다. BeanDefinition을 바탕으로 객체를 만들고, 의존성을 연결하고, 생명주기를 관리한다.

#### BeanFactory
Spring Container의 가장 기본적인 형태
BeanFactory = Bean을 생성하고 요청하면 반환해주는 것
1. 객체를 만들고
2. 저장하고
3. 필요할 때 반환

#### BeanDefinition
Spring은 클래스를 바로 객체로 만드는 것이 아니라, 먼저 그 클래스를 "Bean으로 어떻게 만들지"에 대한 설계도를 만든다. 그 설계도가 BeanDefinition이다.

예를 들어,
``` java
@Service
public lcass UserService {
	private final UserRepository userRepository;
}
```

위의 클래스는 Spring의 내부적으로 대략 아래와 같이 정보를 가진다.
BeanDefinition
├── bean name: userService
├── class: UserService
├── scope: singleton
├── dependency: userRepository
├── init method
└── destroy method

즉, BeanDefinition은 "객체 자체"가 아니라 "객체를 만들기 위한 메타정보"입니다.

#### Dependency Injection
예를 들어,
``` java
@Service
public class  UserService {
	private final UserRepository userRepository;
	
	public UserService(UserRepository userRepository) {
		this.userRepository = userRepository;
	}
}
```

Spring은 다음 순서로 본다.
1. UserService를 만들려면 UserRepository가 필요하군
2. UserRepository Bean이 등록되어 있나?
3. 먼저 UserRepository를 준비
4. UserService 생성자에 넣어서 생성
5. UserService를 Bean으로 등록

## spring-context

### 역할
spring-context는 "spring-core와 spring-beans 기반 위에 만들어진 고급 컨테이너 기능이다"
spring-context 모듈의 핵심은 ApplicationContext 이다.

#### BeanFactory와 ApplicationContext의 차이
- BeanFactory는 기본 Bean 공장
- ApplicationContext는 BeanFactory + 실무용 고급 기능

ApplicationContext는 Bean 관리뿐 아니라 다음 기능을 추가로 제공한다.
ApplicationContext
├── Bean 관리
├── Resource 로딩
├── MessageSource, 국제화
├── ApplicationEvent 이벤트
├── Environment, Profile
├── Annotation 기반 설정
└── 다른 Spring 모듈과의 통합

우리가 흔히 쓰는 컨테이너는 대부분 ApplicationContext이다.
예를 들어 Spring Boot에서 애플리케이션을 실행하면 내부적으로 ApplicationContext가 만들어진다.
``` java
SpringApplication.run(MyApplication.class, args);
```
이 한줄은 단순 실행처럼 보이지만, 내부적으로는 대략 아래와 같은 일이 발생한다.
1. ApplicationContext 생성
2. 설정 클래스 읽기
3. Component Scan
4. BeanDefinition 등록
5. Bean 생성
6. 의존성 주입
7. 애플리케이션 실행 준비 완료

#### @Component, @Service, @Repository는 어디에 관련하나?
애노테이션 기반 개발은 spring-context와 밀접하게 관련된다. spring-beans가 "Bean을 만들고 관리하는 엔진"이고 spring-context가 "애노테이션, 스캔, ApplicationContext 등 실무형 컨테이너 기능"이다.

spring-context는 BeanFactory를 실무에서 쓰기 편한 ApplicationContext로 확장한 모듈이다. 우리가 일반적으로 말하는 "Spring Container"는 대부분 ApplicationContext를 가르킨다.

## spring-expression

### 역할
Spring Expression Language를 담당한다. 예를 들어 아래와 같은 Expression을 처리한다.
``` java
@Value("#{2 + 3}")
private int number;

@Value("#{systemProperties['user.home']}")
private String home;
```
또는 설정에서 조건이나 값을 동적으로 평가할 때 사용된다.
공식 문서에 따르면 SpEL은 런타임에 object graph를 조회하거나 조작할 수 있는 표현식 언어이며, property 접근, method invocation, collection 접근, 논리/산술 연산 등을 지원한다.

SpEL의 위치
Core / Beans / Context는 Container의 핵심 뼈대이다.
하지만, Expression은 설정과 Bean 처리 과정에서 쓰일 수 있는 보조 기능이다.

Core Container 관점에서 보면
spring-core
→ 클래스 읽기, 리소스 처리, 내부 유틸리티 지원

spring-context
→ @Configuration, @ComponentScan 처리
→ ApplicationContext 생성

spring-beans
→ OrderService BeanDefinition 등록
→ OrderRepository 의존성 확인
→ 생성자 주입
→ singleton Bean으로 관리

spring-expression
→ @Value("#{...}") 같은 표현식이 있으면 평가

즉, 단순히 @Service 하나를 붙였을 때도 내부적으로 여러 모듈이 협력한다.


Core Container= Spring이 객체를 만들고, 보관하고, 연결하고, 꺼내 쓰게 해주는 핵심 시스템

그리고 그 안의 모듈은:

|모듈|핵심 역할|기억 문장|
|---|---|---|
|`spring-core`|기본 기반 기능|Spring 전체의 공통 도구 상자|
|`spring-beans`|Bean 관리|BeanDefinition과 BeanFactory의 핵심 엔진|
|`spring-context`|고급 컨테이너|ApplicationContext와 애노테이션 기반 실무 기능|
|`spring-expression`|표현식 처리|런타임에 값을 계산하는 SpEL 모듈|

제일 중요한 건

Spring Container를 진짜 움직이는 중심은 `spring-beans`이고, 우리가 실무에서 만나는 컨테이너 형태는 `spring-context`의 `ApplicationContext`입니다.