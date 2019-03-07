MVP 모델은 Model-View-Presenter 로 구성 

### 뷰(View)

The view is a passive interface that displays data (the model) and routes user commands (events) to the presenter to act upon that data.
실제 view 에 대한 직접적인 접근을 담당합니다. 안드로이드에서 액티비티/프래그먼트는 뷰의 일부로 정의합니다.  
View 에서 발생하는 이벤트는 직접 핸들링 할 수 있으나 Presenter 에 위임하도록 합니다. 위임하는 방법은 액티비티가 뷰 인터페이스를 구현해서 Presenter에서 코드를 만들 인터페이스를 갖도록 하면 됩니다. 이렇게 하면 특정 뷰와 결합되지 않고 가상 뷰를 구현해서 간단한 유닛 테스트를 실행할 수 있습니다. 

### 프리젠터(Presenter)

The presenter acts upon the model and the view. It retrieves data from repositories (the model), and formats it for display in the view
본질적으로는 MVC의 컨트롤러와 같지만, 뷰에 연결되는 것이 아니라 인터페이스로 연결된다는 점이 다릅니다. 이에 따라 MVC가 가진 테스트 가능성 문제와 함께 모듈화/유연성 문제 역시 해결합니다. 프리젠터(Presenter)의 역할을 한줄로 표현한다면 뷰(View)와 모델(Model) 사이에서 자료 전달 역할을 합니다. 

### 모델(Model)

The model is an interface defining the data to be displayed or otherwise acted upon in the user interface.
앱 데이터 및 상태에 대한 비지니스 로직을 수행합니다.

*비즈니스 로직(Business logic)은 컴퓨터 프로그램의 규칙에 따라 데이터를 생성·표시·저장·변경하는 부분을 말합니다. 데이터베이스, 표시장치 등 프로그램의 다른 부분과 대조되는 개념으로 쓰입니다.

MVP도 여러 형태가 있고 구현 방식도 다양해서 모호함. 중요한 건 아래 목적에 맞게 구현하는 것.

1. View를 뷰와의 상호작용으로 시작되는 비지니스 로직(Presenter)을 분리. 뷰에서 보여지는 데이터 또한 Model이라는 모듈로 분리.
2. View와 Model의 상호작용은 Presenter를 통해서. View와 Model은 서로를 참조하지 않는다.
3. 이러한 패턴을 적용하면 하나의 component를 변경할 때 다른 component의 변경 없이 가능하다. 다른 View나 Model로 쉽게 교체할 수 있다. 예를 들면 content provider를 통해 리스트의 아이템을 가져오던 방식에서 SQLite에서 리스트의 아이템을 가져오도록 쉽게 변경할 수 있다. 물론 다른 component의 변경 없이.
4. 테스트 자동화할 수 있는 코드가 매우 늘어난다.

