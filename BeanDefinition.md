
Bean을 만들기 위한 설계도

- BeanDefinition은 스프링이 XML, Java 코드 등 다양한 설정 형식을 지원할 수 있도록 Bean 설정 정보를 추상화 한 Spring container의 핵심 메타데이터
- 즉, 인터페이스
- 주 책임 모듈은 spring-beans
- 관련 모듈은 spring-context

Spring은 클래스를 보고 바로 객체부터 만들지 않는다. 먼저 아래와 같은 정보를 만든다.

- 이 Bean은 어떤 클래스로 만들까?
- 생성자 인자는 무엇인가?
- scope는 singleton인가 prototype인가?
- 초기화 메서드는 무엇인가?
- destroy 메서드는 무엇인가?
- 의존성은 무엇인가?

이런 정보가 담긴 설계도가 BeanDefinition

## BeanDefinition이 중요한 이유!
Spring이 강력한 이유가 BeanDefinition이기 때문이다.

Bean을
1. XML로 등록을 하든
2. Java Config로 등록하든
3. Component Scan으로 등록하든
결국 Spring 내부에서는 BeanDefinition이라는 공통 형식으로 변환된다(만들어진다).

![[BeanDefinition0]]

자연스럽게 Bean을 등록하는 대표적인 방법  3가지
1. XML
2. Java Config
3. Component Scan

계층 지도의 관점
"Spring Context" creates BeanDefinitions from XML, Java config, and component scanning.
