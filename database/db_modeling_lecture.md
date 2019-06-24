소프트웨어를 해부하면 크게 동작과 데이터로 나눌 수 있다.
DB모델링을 해야 하는 이유? 데이터를 잘 다루기 위해서.

모델링? 모델을 보고 닮게 만든 일.

### 모델링
- 만들고자 하는 머릿속 생각을 누구나 볼 수 있게 본을 뜨는 것.
- 객체나 데이터베이스를 그림으로 표현하는 것.

현업에선 보통 3개 씀.
- 시퀀스 다이어그램
- UML
- ERD

논리모델과 물리모델

IE 표기법

Barker 표기법

RDBMS는 데이터 무결성을 준수한다.
(NoSQL는 데이터 무결성을 보장하지 않는다.)

### RDBMS의 무결성

RDBMS 무결성는 아래 3가지 무결성을 충족한다.
- 개체 무결성
- 참조 무결성
- 범위 무결성 : 쉽게 얘기하면 같은 컬럼이면 동일한 데이터 타입만 들어간다.

### 알아야 할 용어
- Identifying Relationship : 식별 관계
- Mandatory : 필수
- Cascade : 연쇄

### Identifying Relationship & Mandatory

Identifying Relationship : A 테이블의 PK가 B 테이블에서도 PK이다.

Mandatory : 내가 태어나기 위해서는 부모님이 계셔야 하지만
Optional : 부모님이 태어나기 위해서 내가 필수적인 건 아니다.

Mandatory : !Null

### Foreign key option
RESTRICT : 참조값이 있으면 에러
CASCADE : 연쇄적으로 참조값이 있는 애들을 다 지움
SETNULL : 참조값을 NULL로 세팅
NOACTION : RESTRICT랑 같음

사용자 흐름대로 데이터베이스를 설계하는게 가장 편했음.
ex) 글을 쓰려면? 회원가입을 먼저해야됨. 그래서 user 테이블 생성.

관행적으로 테이블명은 복수형으로 씀.

테이블 설계시엔 기술적인 차원 뿐만 아니라 기획적인 측면도 고민해야됨.
- ex) username을 PK로 하면 아이디 변경이 불가능함.

보통은 DATETIME보단 TIMESTAMP를 씀. 글로벌 서비스를 생각해서.

글로벌 서비스에서 보통 타임존을 고려하지 않음. 
ex) SNS 포스트의 10초 전. 10분 전. (TIMESTAMP 연산. 서버 입장에서 매우 효율적)

### ERD(Entity-Relation-Diagram)

점선 : Non Identifying Relationship.
실선 : Identifying Relationship일 때. Composite key.

### LINE, 카카오 등 메신저
- 친구 메시지도 내 id 데이터로 등록되어 있음. 방 인원수만큼 같은 메시지가 복사되어 있는 구조.
- 왜? JOIN을 없앨 수 있고, 극단적으로 성능을 높일 수 있음.
- 대신 수정에 대한 비용이 높음(다 수정해줘야 되기 때문). 트위터는 수정이 안 됐었음.


### 정규화 & 역정규화(성능 향상을 위해 데이터 중복을 허용)
- `정규화` : 중복을 최대한 줄임.
- 장바구니 화면을 그리려면 product_in_cart와 goods를 loop로 연산해야 된다.
- product_in_cart에 장바구니 화면을 그리기 위한 goods 필수 데이터(price, name 등)를 중복하여 넣어준다.
- 이렇게 하면 join하지 않고 select 1번으로 줄여서 성능 향상을 달성할 수 있음. 이게 `역정규화`.
- 역정규화란 개념이 먼저 있던게 아니라 성능 향상을 위해 조치를 취하다 보니 이러한 이름을 붙이게 된 것.

과제: 전체적으로 필요한 컬럼 추가. 배송 회사 테이블, 상품 이미지 테이블. 개인 프로젝트 ERD.