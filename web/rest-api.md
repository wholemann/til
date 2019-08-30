## API Versioning

### 대표적인 방법

  - API 수정이나 기능 추가가 불가피 하기 때문에 배포 이후엔 API 버전 관리를 피할 수 없다.
  - 크게 4가지 방법 정도가 나와 있다.

  1. URI versioning
  ```
  HTTP GET:
  https://haveibeenpwned.com/api/v2/breachedaccount/foo
  ```
  2. custom request header
  ```
  HTTP GET:
  https://haveibeenpwned.com/api/breachedaccount/foo
  api-version: 2
  ```
  3. Accept header (Github의 방식)
  ```
  HTTP GET:
  https://haveibeenpwned.com/api/breachedaccount/foo
  Accept: application/vnd.haveibeenpwned.v2+json
  ```
  4. query string
  ```
  https://adventure-works.com/customers/3?version=2
  ```

### 특징

- URI or query string 버저닝은 매번 같은 데이터를 참조하므로 캐싱에 장점이 있다.
- 물론 일부 구형 웹 브라우저와 웹 프록시는 URI에 쿼리 문자열을 포함하는 요청에 대한 응답을 캐싱하지 않는다.
- 다른 두 방법은 헤더값을 검사하기 위한 추가적인 로직이 필요하다.
- 대규모 환경의 경우 서로 다른 버전의 웹 API를 사용하는 많은 클라이언트가 서버 쪽 캐시에 상당한 양의 중복된 데이터를 발생할 수 있다.

### 논란

- Roy fielding에 말에 따르면 `/v1/` 와 같은 버저닝은 바보짓이라고 했다. API가 그만큼 자주 바뀔 일이 없고 다른 것을 리턴하고 싶으면 다른 미디어 타입을 사용하라고 했다.
- 헤더에 버전을 심는 것은 괜찮은 방법이나 조직 내 누군가 빼먹을 가능성도 있다고 한다.
- 위에 제시된 3가지 방법은 모두 문제를 가진 다는 점에서 완벽한 best practice는 없는 듯.

### 의문점 & 감상

- 실제로 REST API는 거의 유니콘과 같은 듯 하다.
- REST 표준을 준수하는 것은 상당히 어렵기 때문에 다들 양심상 RestFul 이라고 얘기 하는 듯 한데 Roy 는 아예 Rest 란 단어도 붙이지 말라고 한다.
- 실제로 첨단 기업들의 API를 봐도 REST하지 않는 곳이 많았다.


### Reference

- https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/
- http://haah.kr/2017/06/26/rest-the-truth/
- https://docs.microsoft.com/ko-kr/azure/architecture/best-practices/api-design#versioning-a-restful-web-api
