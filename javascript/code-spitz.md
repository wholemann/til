## JavaScript Now

ES11(2020) - 완전히 달라짐.

### ECMAScript Standard

- tc39
- 크롬 업데이트 확인 안 할 수 없음.

### ECMAScript 6

- 클래스는 ES5에서 설탕 문법이 아님. 완전 다름.

### ECMAScript 7~10, Stage3

- 주목할 애들
  - 8: shared memory(wasm에서 많이 씀), atomics(저수준 lock)
  - 9: asynchronous iterators(비동기 + iterator)
  - State3(11): top level await

## Program

### Program & Timing

- Language code: Lint & IDE
- Machine language: JVM, 브러우저 등등 언어가 실행되는 환경.
- 설계가 나쁜 신호: 하나의 수정 사항에 여러개의 요소를 건드려야 될 경우.

### JavaScript Timing

- Browser Load, Browser Parsing 을 신경써야 됨.

## Runtime

### Runtime Execution

- 폰 노이만 머신
  - 명령이 순차적으로 실행되는 것 뿐.
  - 메모리에 있는 걸 가져와서 결과를 메모리에 다시 넣는 것.

- instruction: machine이 이해할 수 있는 명령어
- decoding: 메모리엔 효율적으로 저장해놓았기 때문에 해석하기 전에 풀어야 됨.

### Runtime Details

- essential definition loading: 우리 코드보다 더 우선되어야 하는 언어 차원의 라이브러리, 근본 요소...
- vtable mapping: 로딩 이후에나 실제 메모리는 건들 수 있기 때문에 컴파일러가 가상으로 돌려보고 실제 메모리에 매핑.
- run
- runtime definition loading -> run -> runtime definition loading -> run... (Runtime)
- Browser load, Browser parsing, Run (Runtime)
- javascript에선 declare time, runtime 구분이 중요함. 시점에 따라 상대적인 개념.

## State Control

### Directive Reference

- 참조 변수가 외부에 공개되면 변경여부에 따라 추적하기가 어려움

### Indirective Reference

- b.target = &d ( '.'연산은 runtime에 메모리를 한번 더 참조하는 연산. 대신 우리의 멘탈 모델이 깨지지 않음)
- 링크드 리스트가 결국 현대 언어에서 상태관리에 가장 큰 핵심.

## Flow Control

### Sync flow control

- Sync flow의 핵심은 메모리의 적재된 값(state)에 달려있음.
- Sub Flow(sub routine).

### Blocking

- Sync Flow가 실행되는 동안 다른 일을 할 수 없는 현상.
- 사실 Non blocking 코드는 노이만 머신에서 존재하지 않음.
- Blocking 구간을 줄이는 것으로 타협 -> sync flow 짧게 하기.
- 다른 쓰레드에 syncflow를 떠넘기기.
- 메인 쓰레드는 OS가 점유해야 하기 때문에 메인 쓰레드는 조금만 쓰길 권장함.
- 그래서 내 프로그램이 잘 돌려면 Blocking부분을 다른 쓰레드로 보내야 함. OS가 우대해줌.

- Non Blocking: Sync Flow가 납득할 만한 시간 내에 종료되는 것.
- Non Blocking은 지향할 수 있을 뿐. 달성할 순 없음. 존재하지 않기 때문.

## Async

### Sync & Async (위의 Sync와는 의미가 다름)

- Sync: 서브루틴이 즉시 값을 반환함. (return을 이용해서 값을 반환)
- Async: 서브루틴이 다른 수단으로 값을 반환함. (return한 Promise는 값이 아니기 때문. Promise는 즉시 쓸 수 없는 값.)
- 다른 수단: Promise, callback function, iterations

### Async 단점

- 우리 사고 체계가 익숙하지 않음. blocking에 맞게 굳어짐.

### Sync의 장점 + Async의 장점

- continuation
- 이를 활용하는 프로그래밍 스타일: Continuation Passing Style


