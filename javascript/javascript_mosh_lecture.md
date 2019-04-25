### best practive about variable
- 보통 string엔 '' single quotes를 쓴다.
- 변수는 각각 single line에 선언한다.

Dynamic language = runtime에 변수 type이 결정된다.
Static language = compile time에 변수 type이 결정된다.

### Primitives/Value Types
String, Number, Boolean, Symbol, undefined, null

### Reference Types
Object, Array, Function


### Array
- JavaScript는 dynamic language기 때문에 array의 길이 또한 동적으로 변할 수 있다.
- 다른 언어와 달리 array 안의 요소들의 type이 달라도 된다. ex) ["red", "blue", 1]

### Equality Operators
```
// Strict Equality (Type + Value)
// 대부분 === 를 쓴다.
1 === 1   // true
'1' === 1   // false

// Lose Equality (Value)
// 타입이 다를 경우 왼쪽 타입에 오른쪽 타입을 맞춘다.
1 == 1    // true
'1' == 1    // true
true == 1   // true
```

### Logical Operators
Falsy (false) : undefined, null, 0, false, '', NaN
Anything that is not Falsy -> Truthy
```
false || 'Mosh'   // "Mosh"
false || 1    // 1
false || 1 || 2   // 1 (Short-circuiting)
```
Short-circuiting: 중간에 1을 읽는 순간 Truthy로 판별하여 이 후에 expression은 읽을 필요가 없다.

```
let userColor = undefined;
let defaultColor = 'blue';
let currentColor = userColor || defaultColor;   // blue
```

userColor는 falsy이기 때문에 그 다음 나오는 defaultColor(non boolean)의 값이 나옴.


### Bitwise Operators
'|', '&'
```
// Read, Write, Execute
// 00000100
// 00000010
// 00000001

const readPermission = 4;
const writePermission = 2;
const executePermission = 1;

let myPermission = 0;
myPermission = myPermission | readPermission | writePermission;
console.log(myPermission);

let message = 
  (myPermission & readPermission) ? 'yes' : 'no';

console.log(message);   // yes
```

### for-in, for-of
- for in은 객체들의 속성을 반복하여 작업을 수행한다. 즉, key값에만 접근할 수 있다.
- for of는 컬렉션 전용 반복 구문이다. array의 value를 직접 가져올 수 있다.
```

// for-in
const person = {
  name: 'Mosh',
  age: 30
};

for (let key in person)
  console.log(key, person[key]);


// for-of
const colors = ['red', 'green', 'blue'];

for (let color of colors)
  console.log(color);
```


### Object
```
const circle = {
  radius: 1,
  location: {
    x: 1,
    y: 1
  },
  isVisible: true,
  draw: function() {
    console.log('draw');
  }
};

circle.draw();    // Method
```

### Factory Function, Constructor Function
- key와 value가 같을 경우 key 생략 가능하다.
- object 내부에선 function 키워드도 생략 가능하다.
- 커뮤니티에서 의견이 분분하지만 두 패턴은 차이가 없으므로 선택은 취향이다.

```
// Factory Function
// Camel Notation: oneTwoThreeFour

function createCircle(radius) {
  return {
    radius,
    draw() {
      console.log('draw');
    }
  };
}

// Constructor Function
// Pascal Notation: OneTwoThreeFour

function Circle(radius) {
  this.radius = radius;
  this.draw = function() {
    console.log('draw');
  }
}

// new는 empty object '{}' 를 생성
// 그 후 생성자 function를 호출하여 object 리턴
const circle = new Circle(1);
circle.draw();
```

### Dynamic Nature of Objects
- const는 재할당이 불가능할 뿐이다.
- const로 선언한 Object는 속성을 추가하거나 삭제가 가능하다.
```
const circle = {
  radius: 1
};

circle = {};  // TypeError!

circle.color = 'yellow';
circle.draw = function() {}

delete circle.color;
delete circle.draw;

console.log(circle);
```

### Constructor Property
- 사실, 객체 생성시 JavaScript 엔진이 내부적으로 생성자를 실행한다.
- 생성자 function을 직접 작성하지 않았을 땐 빈 생성자가 실행된다.
```
let x = {};   // let x = new Object();
new String();   // '', "", ``
new Boolean();    // true, false
new Number();   // 1, 2, 3, ...
```


### Functions are Objects
- JavaScript에서 함수는 객체(1급객체)다.
- Circle function은 내부적으로 new Function으로 표현된다.

```
function Circle(radius) {
  this.radius = radius;
  this.draw = function() {
    console.log('draw');
  }
}

const Circle1 = new Function('radius', `
this.radius = radius;
  this.draw = function() {
    console.log('draw');
  }
`);

const circle = new Circle1(1);
```

- call/apply는 첫번째 인자를 호출되는 함수의 this에 바인딩시킨다.
- apply는 배열을 인자로 전달할 수 있다.
- 빈 객체 {}에는 radius 프로퍼티가 없으므로 radius 프로퍼티가 동적으로 추가 되고 값이 할당된다.

```
function Circle(radius) {
  this.radius = radius;
  this.draw = function() {
    console.log('draw');
  }
}

Circle.call({}, 1);
Circle.apply({}, [1, 2, 3]);

const another = new Circle(1);    // Circle.call({}, 1); 과 같다
```

### Value vs Reference Types
- 자바랑 비슷한 것 같다.
- primitives는 값을 복사하고 objects는 reference(참조: 메모리주소)를 복사한다.

