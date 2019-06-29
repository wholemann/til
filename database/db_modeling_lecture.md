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


이미지 저장법
user_id.jpg
fastbook.com/images/0001.jpg

url로 저장하면 나중에 서버 확장하기 쉬움
img1.fastbook.com
img2.fastbook.com

브라우저에 캐싱되기 때문에 파일명이 같아서 이미지가 바뀌어도 변경이 안 될 수 있음.
그래서 보통 갱신을 위해 (user_id).jpg?20190629 과 같은 query string을 붙여서 저장한다.

상의 0001 티 0002 반팔 0009 어린이 0008 반팔 0010

gooods에 category_tag 개념으로 #0001 #0002 #0009 #0008 #0010 을 넣어서

복수의 카테고리에 해당된 상품 N:M 관계를 해결하며 중계 테이블도 없어도 됨.

검색은 0010을 해버리면 반팔인 상품 모두 검색 가능.

user = 1천만
post = 10억개

SELECT * FROM post WHERE LIMIT 100000, 100

LIMIT 연산은 100100개를 가져와서 10만개 버리고 100개 보여줌.

SELECT * FROM post WHERE id > 100000 LIMIT 100
절대값 10만으로 가져오면 중간에 삭제된 경우 정확한 처리가 안 됨.

상대값 을 따로 컬럼으로 관리하면 해결 가능. but, 매번 update를 해줘야 함.

과연 10만번째 페이지를 보는 경우가 많을 것인가? 글삭제가 많을 것인가? 는 고민해봐야 됨.

사실은 이렇게 규모가 커지면 페이징 처리를 안 해도 되도록 UX를 유도함.

INDEX?
데이터의 목차 생성

쿼리 튜닝
SELECT * FROM post WHERE user_id = 10 ORDER BY id DESC INDEX(user_id, id)

쿼리 튜닝은 검색의 범위를 좁히는 방향으로 해야한다.
-> WHERE location=서울 AND sex=FEMALE (O)
sex=FEMALE AND location=서울(X) sex의 모수가 크기 때문.

분포 비율을 생각하며 짜야 됨.

INDEX(location, sex) 를 해줘야 됨.
쿼리를 location부터 날리기 때문.

근데 사실 sex는 인덱스 걸어봤자 50%라 효용이 없음.
EXPLAIN(실행계획 분석)을 통해 분석해보고 결정하는게 좋음.


컬럼 배치 순서도 속도에 관련이 있음.
id, content, created_date, updated_date, user_id ... 면 3칸을 점프해야됨.
id, user_id, content, created_date,...

index를 걸지 않은 컬럼의 경우 WHERE 절에서 컬럼끼리 거리가 가까울수록 빠름.


