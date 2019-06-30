#### Facts:

- javascript object {key: value}에서 키 값으로 함수의 연산결과를 대입하고 싶었으나 문법 오류.
- 한참 이것 저것 해보다가 키값 동적 할당 등 다양한 검색어를 때리다가 computed property name을 발견.
- ES6는 computed property name를 아래와 같이 지원함.
```
const a = {
  ['key' + i++]: value
}

```

#### Feelings:
- computed...computed 왜 생각을 못했나...

#### Findings:

- 속성 이름을 동적으로 넣고 싶은 거였다.
- 영어로 하면 computed property name이다. 사실 영어가 익숙해도 쉽게 떠올릴 수 있을진 모르겠다.

#### Future Action Plan:

- 이런건 자주 쓰이는 표현이니 외워야 되는 듯 하다.
- 일단 문제가 생기면 해당 이슈를 표현하는게 영어로 어떤 표현일지 최대한 생각해보고 검색하자.

#### Feedback: