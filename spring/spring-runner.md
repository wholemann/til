# 1일차

## Spring Webflux

- 솔직히 한국에선 라인 정도 빼고는 쓸 일이 없다.
- 엄청난 트래픽을 감당하기 위한 기술.

## 엔터프라이즈 서비스 기능

- Java EE에 정의된 기능.

## AOP

- 비지니스 로직 외에 외부에서 처리되는 부분.

## Spring MVC

- CoC는 어노테이션 등장 이후 쓰이지 않음.
- Spring MVC는 Blocking I/O가 있어서 Reactive는 고민을 많이 해야됨.
- Blocking이 언제 끼어들지 알 수 없음.

## Clean Architecture

- 정책 = 비지니스 규칙.

## 스프링 부트와 로깅 전략

- 백기선님 영상과 자료 참고

## View, View Resolver

- 뷰를 처리할 때 이 두개가 협력해서 처리함.
- view Resolver의 메소드가 호출될 때 접두사, 꼬릿말이 붙는다.
- dispatcher servlet가 view Resolver에게 물어봄.

## Spring 의존성 주입

- setter, 생성자를 이용함.
- field injection은 안티 패턴. 테스트가 힘들어짐.

## request handler

- handler adapter: handler를 호출.
- 핸들러가 반환하는게 문자열이면 뷰의 이름이다.
- url의 이름이 곧 view이름으로 취급되어 void로 반환해도 됨.

## Controller는 Application의 인터페이스에 의존함

- Controller가 TodoFinder에 의존.
- TodoFiner의 구현체인 TodoManager의 변경이 Controller에 영향을 주지 않음.

## InitializingBean, CommandLineRunner, ApplicationRunner

- 3가지 중 상황에 맞게 적절히 골라쓰면 된다.
- CommandLineRunner가 먼저 나왔는데 쓰기 불편해서 ApplicationRunner가 나왔다.

## Command

- Java Bean 규약을 지켜서 만들어 줘야 됨.
- getter, setter를 가진 객체.

## Bean Validator

- Java Bean의 Validation에 대한 표준.
- 구현체는 Hibernate Validator.
- api에서 입력값 validation을 해주고, domain 모델에서도 validation을 해줘야 함.
- 업무규칙은 domain 레이어에서 구현해야됨.

## 타입에 대한 변환

- Servlet 스펙상 문자열 아니면 Binary 데이터 뿐.
- Spring이 내부적으로 타입 변환 스펙을 가지고 있음.

# 2일차

## Error 처리

- 중앙으로 던지는 것. 비지니스 로직 내에서 처리하는 것.
- 사용자가 보기 편한 것과 개발자가 보기 편한 에러 메시지는 다름.
- 등록된 에러처리 Bean이 없으면 기본 에러처리를 선택하는 전략.
- @Bean = 컨테이너에 등록된 관리대상(스프링 입장에선 Component, Controller 모두 Bean)

## 로그인

- Spring MVC는 서블릿을 추상화 시켰기 때문에 Serlet API 사용하는 걸 지양해야됨.
- 종속적인 코드는 테스트가 어려워짐.
- Java Bean 규약을 지킨다는 것 = getter setter를 갖춘 클래스
- 각 ExceptionHandler의 스코프는 선언된 해당 핸들러가 선언된 Controller에 한정됨.
- BindingResult는 개발자가 에러를 핸들링하겠다는 선언.
- [session-management-using-spring-session](https://sivalabs.in/2018/02/session-management-using-spring-session-jdbc-datastore/)

## 세션

- 매번 UserSession Repository에서 get하기 보다는 핸들러 한 곳에서 처리하도록 함.
- filter는 스프링이 생성하지만 사용은 컨테이너에게 위임.
- UserSessionFilter에게 인증 방식을 책임지게 하면서 RolesVerifyHandlerInterceptor는 세션과 관련이 없게 됨.
- RolesVerifyHandlerInterceptor는 Session을 쓰든 JWT를 쓰든 관심이 없다.

## 프로필 이미지 저장

- 영구 저장을 위해 확장성을 고려해야됨.
- 해당 어플리케이션 서버가 아닌 다른 외부 저장소를 이용할 수도 있기 때문.
- 그래서 추상화가 필요하고 영구 저장을 위한 인터페이스가 필요함.