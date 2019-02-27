# Android Architecture

Last Edited: Feb 21, 2019 6:07 PM
Tags: Android

Section 1

Software Architecture

- Risk management decisions in anticipation of inevitable change of system's requirements
    - 좋은 아키텍쳐는 변화에 탄력적으로 대응할 수 있음
- Good architecture is a result of many educated trade-offs
    - 좋은 아키텍쳐는 많은 삽질을 통해 얻을 수 있음
- Mostly independent of third-party frameworks and libraries
- Good architecture severely reduces the cost(effort) of software development and maintenance

Section 2

UI Layer

- UI 요소로 유저와 인터렉션을 하고 그걸 시스템으로 보냄

Application Layer

- 시스템의 결과를 UI Layer로 보냄
- UI Layer와 유저의 인터렉션을 조정

Domain Layer

- 흔히 말하는 비지니스 로직
- 커머셜 서비를 구현한다면 장바구니 담기나 결제 같은 것

Infrastructure Layer

- 도메인에 한정되지 않는 제너럴한 기능들을 제공함
- 네트워크 라이브러리, 데이터베이스, 이벤트 버스 같은 것들

Presentation Layer = UI Layer + Application Layer

MVx

- 여기서 모델과 x가 명확하지 않아서 혼란을 주고 일관성이 없음
- 어떤 패턴은 모델이 비지니스 로직이라고 정의하는 반면, 어떤 패턴은 모델은 어플리케이션의 상태라고 정의, 또 다른 건 단순히 데이터 구조라고 정의
- 단지, View가 사용자 인터페이스란 점은 모두의 공통점

Section 3

Google 공식 Android Architecture Blueprint가 최적은 아님

일부 잘못된 부분이 있고 결함이 있음(원칙과 다르다는 얘기인 듯)

MVP 예제
([https://github.com/googlesamples/android-architecture/tree/todo-mvp/](https://github.com/googlesamples/android-architecture/tree/todo-mvp/))

View

- View 인터페이스 안에 뷰가 해야될 일들(메소드)이 정의되어 있음
- Presenter가 UI 갱신할 때 호출할 듯

Presenter

- result 이건 뭐지? 아마 startActivityResult 같은 거 인듯
- 그 외에 메소드들도 뭔가 이상함
- 본래의 MVP 정의에 따르면 Model과 View 사이의 컨트롤 타워 역할
- 예제의 Presenter는 외부에서 명령을 받는 존재임
- completeTask가 View인 TasksFragment에서 호출됨

종합

- 저 예제의 문제점은 View에 UI Layer Logic 뿐 아니라 Application Layer Logic도 상당 부분 포함됨
- MVP에서 View는 재사용 가능해야 되는데 Presenter와 너무 강하게 엮여 있어서 재사용이 힘듦

Section 4

MVx의 장점?

- 흔히들 테스트 가능한 코드를 MVx 패턴의 장점이라고 함
- 사실 테스트하기 좋은 코드는 부수적인 장점일 뿐
- 가장 큰 장점은 UI 로직의 분리이겨 그게 MVx 패턴의 핵심 지침

UI 로직의 의무

- 시스템의 처리 결과를 렌더링
- UI요소를 통해 유저 인터렉션을 캡쳐하고 시스템에 전달(핸들링하진 않음)

UI 로직의 특징

- 디테일하고 정확한 요구가 많음
- 높은 확률로 변경이 잦음
- 코드를 읽기보단 구현 자체를 봄
- 수동으로 테스트하기 매우 쉬움
- 테스트 자동화하기가 어려움

안드로이드에서 UI 로직 분리가 어려운 이유?

- Activity가 너무 많은 책임을 지고 있음
    - Life-cycle, Fragments, Runtime permissions, Dependency injection...
- 일명 God Object임
- Fragment도 God Object!

UI 로직 분리

- 뷰 인터페이스 구현체로 옮기기
- 액티비티는 뷰 객체를 단순히 참조하는 방식
- MVx에서 x가 액티비티라고 해서 모든 로직을 가지고 있어야 하는 건 아님
- x는 복합적인 다수의 클래스를 이용해서 구현

MVC, MVP, MVVM

- 사실상 MVC, MVP는 같은 패턴
- MVVM은 좀 다른 부분이 있으나 용어 차이가 구조적인 엄청난 차이를 보장하진 않음
- 사실상 MVC로 통일해도 무방

Section 5

View 인터페이스를 만드는게 당장은 귀찮아도 나중 생각하면 훨씬 유지보수 비용 절감

MVC View in ListView

- ViewHolder가 더이상 필요 없음
- messy한 어댑터를 방지할 수 있음
- 특정 어댑터에만 종속되지 않고 어디서든 재사용 가능

Warning 

Be very careful with inheritance. It can be easy to abuse, which will lead to tight coupling, and maintenance issues.

Extract functionality into base classes, only if you are confident that it is general and relevant to all subclasses.

### 왜 안드로이드에서 MVC Views를 안 쓸까?

Disadvantages of Using Android Views as MVC Views

- Inherit thousands of lines of code
- Inherit 100+ methods
- Standalone hierarchy for each specific Android ViewGroup

### Summary of MVC Basics

- MVC views are classes that implement ViewMvc interface
- Interactive MVC views implement ObservableViewMvc interface
- Dedicated interfaces for specific MVC views and their Observers
- MVC views can be nested
- Activities are MVC controllers
- Android Views are non-optimal implementation choice for MVC views

Section 6

### Relationship between fundamental dependency injection techniques and DIAP:

- Different levels of abstraction (class vs application)
- Fundamental techniques – class level Single Responsibility Principle
- DIAP – application level Separation of Concerns
- Implementations of DIAP use fundamental techniques under the hood

### DI에 대한 미신?

- 작은 크기의 앱에는 유용하지 않다?
- 아니다. 모든 경우에 유용하다고 생각한다.

### Activity에서 관심사의 분리 SoC

- Retrofit API 호출 로직을 CompositionRoot 클래스에 옮겼다.
- CompositionRoot는 Application 클래스 onCreate에서 생성.
- Activity는 Retrofit API의 사용법을 모른다. 그냥 호출할 뿐.

Section 7

### Controller?

- 얘네들은 application logic을 포함한다.
- user interaction을 다루거나 system output을 view로 보내거나 user flow를 컨트롤한다.
- 네트워크를 다루는 건 controller의 의무가 아니다.
- 보통 controller는 데이터가 어디에서 오는지 전혀 몰라야한다.
- data schema 또한 네트워크에 관련된 로직이므로 Controller 로직에 노출될 필요가 없다.

### Use Case

- application flow와 processes를 encapsulate한 오브젝트다.
- 오직 하나의 single flow를 다룬다.(예를들어, 서버 리퀘스트 -> 데이터 변환 -> notify)
- 계산, 블루투스 통신, 디바이스 상태 observe 등 다양한 케이스로 활용할 수 있다.

