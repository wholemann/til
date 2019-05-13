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

### Object.keys(obj) & Object.entries(obj) & if (key in obj)
- Object의 경우 iterable 하지 않아서 for of를 쓸 수 없으나 Object.keys()를 쓰면 가능하다.
- Object의 key값들을 Array로 리턴한다.
```
for (let key of Object.keys(circle))
  console.log(key);
```
- Object.entries(obj)를 이용하면 key, value를 [['key', value],['key', value],...] 형태로 얻어올 수 있다.
```
for (let entry of Object.entries(circle))
  console.log(entry);
```
- 'key' in obj 는 해당 key가 obj안에 존재하는지 검사할 수 있다.
```
if ('color' in circle) console.log('yes');
```

### Cloning an Object
```
const another = Object.assign({ color: 'yellow' }, circle);
const another = { ...circle };
```

### String
- primitive로 선언하더라도 '.'을 붙이는 순간 자바스크립트 엔진이 내부적으로 String Object로 wrapping 한다.
- 그래서 다양한 메소드를 사용할 수 있는 것이다.
```
// String primitive
const message = "This is my first message';

// String object
const another = new String('hi');
```

### Template Literals
- \n, \' 같은 걸 쓸 필요가 없어진다.
```
const another = `This is my 
'first' message`;

const name = 'John';
const another = 
`Hi ${name} ${2 + 3},

Thank you for joining my mailing list.
`;
```

### Array
- indexOf()는 parameter인 element가 없는 경우 -1이 return
```
numbers.indexOf(1) !== -1   // true
numbers.includes(1)     // true
```
- find()는 array 안에 reference type을 찾기 위해 쓴다.
```
const courses = [
  { id: 1, name: 'a' },
  { id: 2, name: 'b' },
];

const course = courses.find(function(course) {
  return course.name === 'a';
});

// ES6
const course = courses.find(course => course.name === 'a');
```

#### Empty an Array
```
let numbers = [1, 2, 3, 4];
let another = numbers;

// Solution 1 (Recommend)
numbers = [];   // another가 old numbers를 참조할 땐 GC가 작동하지 않음을 유의.

// Solution 2 (Recommend & Mosh's Preference)
numbers.length = 0;

// Solution 3
numbers.splice(0, numbers.length);

// Solution 4   // Performance Cost High. 배열이 크면 클수록 pop()이 여러번 불리기 때문에.
while (numbers.length > 0)
  numbers.pop();
```

#### Combining and Slicing Arrays
- concat할 때 primitive 타입은 새로운 배열에 복사가 일어나지만 reference 타입은 참조 복사가 일어난다.
- 즉, 같은 Object를 가리키게 된다.
```
const first = [1, 2, 3];
const second = [4, 5, 6];

const combined = first.concat(second);

const slice = combined.slice(2);

console.log(combined);
console.log(slice);
```

#### Spread Operator
```
const first = [1, 2, 3];
const second = [4, 5, 6];

const combined = [...first, 'a', ...second, 'b'];

const copy = combined.slice();   // copy origin array to slice array
const copy = [...combined];
```

#### Joining Arrays
- url 만들 때 유용하게 쓸 수 있다. url엔 공백을 넣을 수 없으니
```
const message = 'This is my first message';
const parts = message.split(' ');

const combined = parts.join('-');
```

#### Mapping an Array
```
const numbers = [1, -1, 2, 3];

const filtered = numbers.filter(n => n >= 0);

// arrow function에선 {}가 code block을 나타내기 때문에
// 반환값으로 object를 표현하기 위해선 ()로 감싸줘야한다.
const items = filtered.map(n => ({ value: n }));

console.log(items);

// 여러줄로 chaining method call 하는게 더 깔끔하다.
const items = numbers
            .filter(n => n >= 0)
            .map(n => ({ value: n}))
            .filter(obj => obj.value > 1)
            .map(obj => obj.value);
```


### Functions

#### Function Declarations vs Expressions
- 표현식은 정의하기 전에 호출할 수 없다.
- 자바스크립트 엔진이 코드를 실행할 때 함수 선언식들을 코드 최상단으로 옮기기 때문에 선언식은 호출을 먼저 해도 상관 없다.(Hoisting)
- 그러나 정확히는 이와 같이 작동한다.(https://developer.mozilla.org/ko/docs/Glossary/Hoisting)

```
// Function Declaration
function walk() {
  console.log('walk);
}

run();    // error: run is not defined

// Anonymous Function Expression
const run = function() {
  console.log('run');
};
let move = run; //reference same function
run();
move9);
```

#### Arguments
```
function sum(a, b) {
  return a + b; // 1 + undefined(default)
}
console.log(sum(1));

function sum(a, b) {
  console.log(arguments);   // arguments(Symbol.iterator);
  for (let value of arguments)  //  arguments는 iterator를 가지고 있다.
    total += value;
  return total;
}
console.log(sum(1, 2, 3, 4, 5));
```

#### The Rest Operator
- spread operator랑 형태는 같지만 function의 parameter로 쓰일 때 rest operator라 부른다.
- rest operator는 function의 마지막 parameter로 선언되어야 한다.
```
function sum(...args) {
  console.log(args);   // [1, 2, 3, 4, 5, 10]
  return args.reduce((a, b) => a + b);    // 25
}
console.log(sum(1, 2, 3, 4, 5, 10));

function sum(discount, ...prices) {
  const total = prices.reduce((a, b) => a + b);
  return total * (1 - discount);
}

console.log(sum(0.1, 20, 30));
```

#### Default Parameters
- default parameter 설정 안 해주면 undefined가 된다.
```
function interest(principal, rate = 3.5, years = 5) {
  return principal * rate / 100 * years;
}

console.log(interest(10000));
```

#### Getters and Setters
```
const person = {
  firstName: 'Mosh',
  lastName: 'Hamedani'
  get fullName() {
    return `${person.firstName} ${person.lastName}`;
  },
  set fullName(value) {
    const parts = value.split(' ');
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
};
person.fullName = 'John Smith';

console.log(person);
```

#### Try and Catch
```
const person = {
  firstName: 'Mosh',
  lastName: 'Hamedani'
  get fullName() {
    return `${person.firstName} ${person.lastName}`;
  },
  set fullName(value) {
    if (typeof value !== 'string')
      throw new Error('Value is not a string.');

    const parts = value.split(' ');
    if (parts.length !== 2)
      throw new Error('Enter a first and last name.');
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
};

try {
  person.fullName = null;
}
catch (e) {
  console.log(e);
}

console.log(person);
```

#### Local vs Global Scope
- local 변수는 global 변수보다 block 내에서 우선순위가 높다.
- global 변수나 상수를 선언하는 건 bad practice다.
```
const color = 'red';
function start() {
  const message = 'hi';
  const color = 'blue';
  console.log(color);   // blue

  if (true) {
    const another = 'bye';
  }

  for (let i = 0; i < 5; i++) {

  }
  console.log(i);   // error
}
console.log(message);   // message is not defined
```

#### Let vs Var
- let, const: block-scoped.
- var: function-scoped.
- 암튼 var 쓰지마라.
```
function start() {
  for (var i = 0; i < 5; i++)
    console.log(i);

  console.log(i);   // 0, 1, 2, 3, 4
}
```

#### The this Keyword
- this란 현재 실행하고 있는 function의 object를 가리킨다.
```
// method -> obj
// function -> global (window, global)

const video = {
  title: 'a',
  play() {
    console.log(this);
  }
};

// stop method를 video object에 추가
video.stop = function() {
  console.log(this);
};
video.play();


function Video(title) {
  // 여기서 this는 new로 인해 생성된 {}를 말한다.
  this.title = title;
  console.log(this);
}

const v = new Video('b'); // new는 empty object {} 를 만든다.
```
- 아래에서 forEach 내부의 call back function은 regular function으로 this는 global object를 참조한다.
```
const video = {
  title: 'a',
  tags: ['a', 'b', 'c'],
  showTags() {
    this.tags.forEach(function(tag) {
      console.log(this, tag); // this: global object
    }, this); // this: video object
  }
};

video.showTags();


// 아래와 같이 self를 이용한 방법은 추천하지 않음.
const video = {
  title: 'a',
  tags: ['a', 'b', 'c'],
  showTags() {
    const self = this;
    this.tags.forEach(function(tag) {
      console.log(self.title, tag);
    });
  }
};

function playVideo() {
  console.log(this);
}

playVideo.call({ name: 'Mosh' });   // this가 call에 전달한 parameter를 참조
playVideo.apply({ name: 'Mosh' }, [1, 2]);    // array 전달 가능
playVideo();    // this는 global object

// bind 메소드를 이용. 예전 방식이라 코드 가독성이 좋지 않지만 legacy에선 많이 볼 수 있음.
const video = {
  title: 'a',
  tags: ['a', 'b', 'c'],
  showTags() {
    this.tags.forEach(function(tag) {
      console.log(this.title, tag);
    }.bind(this));
  }
};

// arrow function을 이용. 이게 현대적 방법.
// arrow function에선 this는 containing function(showTags())의 this를 그대로 참조한다.
const video = {
  title: 'a',
  tags: ['a', 'b', 'c'],
  showTags() {
    this.tags.forEach(tag => {
      console.log(this.title, tag);
    });
  }
};
```
