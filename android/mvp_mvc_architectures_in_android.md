## 왜 MVP 와 MVC를 하려고 하는가?

## Model? View? Controller? Presenter?
- 시스템 상태를 저장하는 component는 Model
- 사용자로부터 입출력을 다루는 component는 View
- 시스템의 logical한 기능을 가진 Controller/Prensenter

훌륭한 MVC/MVP 구조라면 이 3가지가 최대한 decoupled 되어야 한다.
뷰를 다른 뷰로 모델을 다른 모델로 교체할 때 각 component간 서로 영향 없이 교체할 수 있어야 한다.

## MVC와 MVP 간의 차이점
안타깝게도, MVC 와 MVP 의 차이에 대한 공식적인 정의는 없다. 인터넷 뒤져봐도 다 제각각임.일단 이러한 모호함에 대해선 생략하자.

1. MVC에선 뷰는 모델 스스로가 알린 상태 변화를 알림받는다. MVP에선 뷰는 모델에 관해선 아무것도 모르며 프레젠터가 모델로부터 뷰에 전달하고 뷰는 이를 통해 뷰에 해당 데이터를 바인드.
2. MVC에선 뷰가 좀 더 많은 로직을 갖는 경향을 갖는데 이는 모델로부터 알림을 다루기 때문이다. MVP에선 MVC에서 뷰가 갖는 위의 로직 부분을 프레젠터가 갖는다. MVP에선 뷰의 역할은 데이터를 뷰로 렌덩링하고 유저 인풋을 캡쳐하는 것이다.

어쩄든 결론만 말하면 MVC는 뷰가 모델의 존재를 알고 MVP는 모델과 뷰는 서로 모르는 사이란 것.

## 안드로이드는 MVC or MVP 를 자체적(native)으로 지원하는가?

M(model)은 가능

## Activity 역할
인터넷에 있는 MVx 패턴의 구현들은 Activity를 뷰로 보며 presenter/controller 기능을 추출하려고 한다. 그건 좋은 접근이 아니며 나는 Activity를 presenter/controller로 보며 뷰 기능을 추출하는 걸 선호한다. 

## MVC or MVP 중에 어떤게 안드로이드 개발에 적합한 패턴인가?
위에서 살펴봤듯 MVP MVC는 매우 비슷하다. 그래서 지금까지도 어떤 아키텍쳐 패턴이 적합한지 안드로이드 프레임워크 측면에서 많은 논의가 되고 있다.

개인적으론 MVP가 더 적합하다고 생각한다. 뷰와 모델을 분리하는게 더 간단하고 깔끔하기 때문.

모델과 뷰가 서로 직접 통신하게 된다면 결국 뷰가 라이프 사이클을 알아야 되는 상황을 직면할 것이다. 이러한 이벤트는 UI 관리와 직접 관련이 없기 때문에, 이러한 이벤트를 뷰가 인식하게 하면 어플리케이션 UI의 추상화가 깨진다.

UI 추상화가 클린코드에 가장 중요한 부분이기 때문에 MVP를 선호한다.

## MVP or MVC
액티비티/프래그먼트(Presenter)와 안드로이드 프레임워크 여러 파트가 강하게 결합되어 있기 때문에 MVP 패턴이 더 적합하다고 위에서 결론이 났다.
MVP와 MVC 용어의 모호함 때문에 많은 개발자들이 두 아키텍쳐 패턴을 구별하지 않는다. 나도 거기에 동의하는 편이다.
controller가 presenter보단 더 descriptive하고 모든 개발 커뮤니티에서도 MVC가 보편적으로 쓰이기 때문에 MVC라 하겠다.(MVP든 MVC든 상관 안한단 얘기)

## MVC view 와 Andrid View 의 차이
- 보통 Android View는 MVC view 가 아님.
- 각 MVC view에 대해, MVC view의 사용자에게 보이는 부분을 나타내는 단일 Android View가 있어야 한다.
- MVC view는 nested MVC views를 포함할 수 있다.

view의 의무: 사용자에게 데이터를 '어떻게' 보여줄지 결정.
controller의 의무: '어떤' 데이터를 보여줄지 결정.

## MVC controller



