소프트웨어를 해부하면 크게 동작과 데이터로 나눌 수 있다.
DB모델링을 해야 하는 이유? 데이터를 잘 다루기 위해서.

모델링? 모델을 보고 닮게 만든 일.

모델링
- 만들고자 하는 머릿속 생각을 누구나 볼 수 있게 본을 뜨는 것.
- 객체나 데이터베이스를 그림으로 표현하는 것.

현업에선 보통 3개 씀.
- 시퀀스 다이어그램
- UML
- ERD

ERD(Entity-Relation-Diagram)

논리모델과 물리모델

IE 표기법

Barker 표기법

RDBMS는 데이터 무결성을 준수한다.
(NoSQL는 데이터 무결성을 보장하지 않는다.)

RDBMS 무결성는 아래 3가지 무결성을 충족한다.
- 개체 무결성
- 참조 무결성
- 범위 무결성 : 쉽게 얘기하면 같은 컬럼이면 동일한 데이터 타입만 들어간다.

알아야 할 용어
- Identifying Relationship : 식별 관계
- Mandatory : 필수
- Cascade : 연쇄

Identifying Relationship : A 테이블의 PK가 B 테이블에서도 PK이다.

Mandatory : 내가 태어나기 위해서는 부모님이 계셔야 하지만
Optional : 부모님이 태어나기 위해서 내가 필수적인 건 아니다.

Mandatory : !Null

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

글로벌 서비스에서 보통 타임존을 고려하지 않음. 10초 전. 10분 전. TIMESTAMP 연산. 서버 입장에서 매우 효율적.

점선 : Non Identifying Relationship.
실선 : Identifying Relationship일 때. Composite key.



