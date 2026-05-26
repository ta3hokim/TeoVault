

모듈이란?
개념적으로 역할을 나눈 논리적 단위
AND
실제로 JAR 파일로 분리된 물질적 단위

모듈은 원래 "복잡한 시스템을 역할별로 분리하기 위한 개념"
예를 들어 운영체제를 모듈 구조로 나누면
OS
 ├── Memory Manager
 ├── File System
 ├── Scheduler
 └── Network Stack
왜 나누냐?
유지보수 쉬움, 독립 개발 가능, 필요한 것만 사용 가능, 책임 분리 가능
스프링도 완전 동일

그러면 Core Container는 정확히 무엇인가?
Core container는 "스프링의 IoC/DI 컨테이너를 구성하는 핵심 모듈 그룹"
Core Container
 ├── spring-core
 ├── spring-beans
 ├── spring-context
 └── spring-expression


![[Pasted image 20260526173328.png]]


Runtime요소의 의미: Spring framework를 기능별로 분류한 것

- 웹 기능
- DB 기능
- AOP 기능
- 메시징 기능

등을 그룹화한 것


[[Core container]]
