### Node
- 브라우저 밖에서 javascript 코드를 동작시킬 수 있는 `runtime 환경`이다.
- Node는 프로그래밍 언어가 아니다.
- V8 engine을 포함한 C++로 작성된 프로그램이다.

#### How Node works
- Non-blocking, Asynchronous
- Node is ideal for I/O-intensive apps
- CPU-intensive apps엔 쓰지마라. Single thread이기 때문.

---

### Node Module System


#### Global Object
- Node에는 window object가 없다. 대신 global object가 있다.
```
setTimeout()
console.log();    // 사실 global.console.log()
```

#### Modules
- file은 모두 module로 취급된다. 변수와 함수는 선언된 module 안에서만 유효하다.
- export를 통해 public하게 만들면 다른 module에서도 참조 가능해진다.
- Node Application은 적어도 1개의 파일(모듈)을 가진다. main module이라 부른다.

#### Loading a Module
- module을 export할 때는 implementaion detail한 부분은 하지 않는다. 캡슐화.
- const에 require로 loading하는게 best practice.

#### Module Wrapper Function
- Node는 모듈의 code를 아래와 같은 function으로 감싸서 실행한다.
```
(function (exports, require, module, __filename, __dirname) {

  var url = 'http://mylogger.io/log';

  function log(message) {
    // Send an HTTP request
    console.log(message);
  }
  
  module.exports = log;  
})
```

#### File System Module
- sync, async 메소드가 있는데 async를 써야한다. Node는 싱글 스레드임을 잊지 말자.

#### Events Module
```
const EventEmitter = require('events');
const emitter = new EventEmitter();

// Register a listener
emitter.on('messageLogged', function () {
  console.log('Listener called');
});

// Raise an event
emitter.emit('messageLogged');
```

#### HTTP Module
- net.Server 를 상속받는다. net.Server는 Event Emitter다.

---

### NPM

#### Package.json
- default 값으로 package.json을 만들어준다.
```
$ npm init --yes
```

#### Installing a Node Package
- --save는 package.json dependencies에 해당 package를 추가한다.
- npm i 의 기본값은 --save이다.
- 해당 패키지의 최신 버전을 다운로드 받는다.
```
$ npm i '패키지 이름' --save
```

#### Using a Package
- require()는 다음과 같이 세가지 방식으로 모듈을 불러온다.
1. Core module에서 해당되는 모듈이 있는지 찾아본다.
2. File or folder에서 해당되는 모듈이 있는지 찾아본다.
3. 위에 해당되지 않으면 node_modules에 해당되는 모듈이 있는지 찾아본다.


#### Package Dependencies
- app이 async 모듈 version1에 의존성을 갖는다면 node_modules에 추가된다.
- 이때 mongoose가 async 모듈 version2에 의존성을 갖는다면 async version2는 mongoose안에 node_modules 안에 추가된다.

#### Semantic Versioning(SemVer)
- 4.13.6 -> Major.Minor.Patch 
- PATCH 버전은 하위호환성을 지키는 범위내에서 버그가 수정된 것을 의미.
- MINOR 버전은 하위호환성을 지키면서 기능이 추가된 것을 의미.
- MAJOR 버전은 API의 호환성이 깨질만한 변경사항을 의미.
- https://docs.npmjs.com/misc/semver
```
틸드(~) 방식 : 현재 지정한 버전의 마지막 자리 내의 범위에서만 자동으로 업데이트.
~1.8.3 = 1.8.x

~0.0.1 : >=0.0.1 <0.1.0
~0.1.1 : >=0.1.1 <0.2.0
~0.1 : >=0.1.0 <0.2.0
~0 : >=0.0 <1.0

캐럿(^) 방식 : 하위호환성이 보장되는 범이 내에선 모두 업데이트 하겠다는 의미.
^4.13.6 = 4.x

^1.0.2 : >=1.0.2 <2.0
^1.0 : >=1.0.0 <2.0
^1 : >=1.0.0 <2.0
```

#### Listing the Installed Packages
- npm list 를 통해 각 패키지의 의존성 트리를 볼 수 있다.
```
$ npm list
$ npm list —depth=0 // 내가 인스톨한 패키지만 보여준다.
```

#### Viewing Registry Info for a Package
```
// package.json 파일의 내용을 볼 수 있도록 표시
$ npm view mongoose

// package.json에서 특정 부분만 볼 수 있도록 표시
$ npm view mongoose dependencies
```

#### Installing a Specific Version of a Package
```
$ npm view mongoose versions  // version 리스팅

$ npm i mongoose@2.4.2  // 특정 버전 인스톨
```

#### Updating Local Packages
- Current는 현재 버전.
- Wanted는 하위호환성이 보장되는 가장 마지막 버전을 의미한다.
- npm-check-updates는 package.json의 dependencies와 devDependencies에 기록되어 있는 각 패키지들을 현재기준으로 최신버전으로 업데이트를 시켜 준다.
```
$ npm outdated

$ npm update    // Wanted 버전으로 업데이트 된다.

$ sudo npm i -g npm-check-updates
$ ncu -u    // package.json의 dependency를 new version으로 업데이트한다.
$ npm i     // 업데이트 된 package.json의 depencency를 참고하여 패키지를 업데이트한다.
```

#### Uninstalling a Package
```
$ npm un mongoose
```

#### Working with Global Packages
```
$ npm i mongoose -g
$ npm -g outdated
```

#### Publishing a Package
- npm 로그인 후 해당 디렉토리에서 npm publish
```
$ npm login
$ npm publish
```

#### Updating a Published Package
```
$ npm version patch // 1.0.0 -> 1.0.1
$ npm version minor // 1.0.0 -> 1.1.0
$ npm version major // 1.0.0 -> 2.0.0

$ npm publish
```


### Building RESTful API's Using Express

#### RESTful Services
- GET : get
- POST : create
- PUT : update
- DELETE : delete
```
// customers 리스트 정보를 response로 받는다.
GET   /api/customers
// id=1인 customer의 정보를 response로 받는다.
GET   /api/customers/1
PUT   /api/customers/1
DELETE    /api/customers/1
POST    /api/customers
```

#### Input Validation
- Best Pracice: Client가 보내는 것을 절대 믿지 않는다.
- Joi와 같은 Validation library를 쓰면 편하다.

#### Handling HTTP PUT Requests
- Object destructure를 이용하면 더 깔끔한 코드로 표현이 가능하다.
```
const result = validateCourse(req.body);
const { error } = validateCourse(req.body); // result.error
```

#### Middleware
- 미들웨어란? 요청에 대한 응답 과정 중간에 껴서 어떠한 동작을 해주는 프로그램이다.
- 미들웨어에서 클라이언트의 요청에 대한 응답을 할 수도 있고, 그 다음 미들웨어로 넘길 수도 있다.
- Request Processing Pipeline
  - Request -> [ json() -> route() ] -> Response
- Custom Middleware function도 만들 수 있다.
- 해당 미들웨어에서 request-response cycle 을 종료하지 않는다면 반드시 next()를 해줘야 한다.
- middleware function은 req, res, next를 인자로 갖는다.
```
function log (req, res, next) {
  console.log('Logging...');
  next();   // pass to next middleware function
}
```


#### Built-in Middleware
- express.urlencoded(): x-www-form-urlencoded으로 전송된 payload를 해석할 때 필요하다.
- express.static(''): ''안에 지정된 디렉토리의 정적 파일을 라우팅 해준다. route url로 바로 접근 가능.

#### Environments
- 개발 환경일 때만 or 프로덕션 환경일 때만 사용하고 싶은 미들웨어가 있을 수 있다. ex) Morgan과 같은 로깅 처리.
- app.get('env')은 process.env.NODE_ENV를 얻어올 수 있다. process.env.NODE_ENV가 설정되지 않았다면 디폴트는 development다.
- development, staging, production 머신 별로 구분해서 동작하도록 할 수 있다.

#### Configuration
- npm i config 를 이용하면 json파일을 이용해서 config 세팅을 관리할 수 있다.
- DB 패스워드, 메일서버 패스워드 등 외부로 노출되면 안 되는 사항을 json파일에 저장하고 레포지터리에 공개하면 문제가 생긴다.
- custom-environment-variables.json을 생성하고 터미널에서 외부 환경변수로 주입해서 처리한다.
```
// 다른 어플리케이션의 환경 변수 설정과 충돌이 일어날 수 있기 때문에 prefix를 붙여서 지정한다.
$ export app_password=1234
```

#### Debugging
- 개발 중에 db에 관련된 로그만 보고 싶을 수도 있고 다른 부분에 관련된 로그만 보고 싶을 수도 있다.
- 그럴 때 debug package를 쓰면 외부 환경 변수 주입으로 분기 처리가 가능하다.
```
const startupDebugger = require('debug')('app:startup');
const dbDebugger = require('debug')('app:db');

$ DEBUG=app:* nodemon index.js
$ DEBUG=app:db nodemon index.js
```

#### Templating Engines
- html을 클라이언트에 return 해야될 때가 있다.
- 그럴 때 쓰는 것이 templating engine이다. 대표적으로 Pug, Mustache, EJS.

### Asynchronous JavaScript

#### Synchronous vs Asynchronous
- Asynchronous는 concurrent, multithread를 의미하지 않는다.
- 식당 종업원이 한명이어도 Asynchronous하게 일을 처리할 수 있음을 생각해보면 된다.
- Asynchronous 처리엔 크게 3가지 방법이 있다. Callback, Promise, Async/Await.

#### Callback Hell
- 콜백 함수 부분을 Anonymous Function에서 Named Function 으로 바꿔서 해결이 어느 정도 된다. 적어도 콜백 헬은 없으나 읽기 좋진 않다.


#### Promise
- Promise? Holds the eventual result of an asynchronous operation.
- 즉, 비동기 조작의 최종 완료나 실패를 표현해주는 객체이다. Promise에 결과값이나 에러가 담긴다.
- 총 3가지 state를 갖는다.
- Pending -> Fulfilled (value)
- Pending -> Rejected (error)

#### Creating Settled Promises
- unit test시에 쓰기 좋다. 이미 resolve 됐거나 reject 됐다고 설정한다.
```
const p = Promise.resolve({ id: 1 });

// 에러 call stack을 보기 위해서라도 Error obect를 쓰는게 best practice
const p = Promise.reject(new Error('reason for rejection...'));
p.catch(error => console.log(error));
```

#### Running Promises in Parallel
- Promise.all은 프로미스가 담겨 있는 배열 등의 이터러블을 인자로 전달 받는다.
- 전달받은 모든 프로미스를 병렬로 처리한다. 이때 모든 프라미스의 처리가 종료될 때까지 기다린 후 아래와 모든 처리 결과를 resolve 또는 reject한다.
- 첫번째 프로미스가 가장 나중에 처리되어도 Promise.all 메소드가 반환하는 프로미스는 첫번째 프로미스가 resolve한 처리 결과부터 차례대로 배열에 담아 그 배열을 resolve하는 새로운 프로미스를 반환한다. 즉, 처리 순서가 보장된다.
- 프로미스의 처리가 하나라도 실패하면 가장 먼저 실패한 프로미스가 reject한 에러를 reject하는 새로운 프로미스를 즉시 반환한다.
- Promise.all 메소드는 전달 받은 이터러블의 요소가 프라미스가 아닌 경우, Promise.resolve 메소드를 통해 프라미스로 래핑된다.
- Promise.race 메소드는 Promise.all 메소드와 동일하게 프로미스가 담겨 있는 배열 등의 이터러블을 인자로 전달 받는다. 
- Promise.race 메소드는 Promise.all 메소드처럼 모든 프라미스를 병렬 처리하는 것이 아니라 가장 먼저 처리된 프라미스가 resolve한 처리 결과를 resolve하는 새로운 프라미스를 반환한다.

#### Async and Await
- Asynchronous한 코드를 Synchronous한 코드처럼 보이게 만들어준다.
- await 키워드는 async 함수에서만 유효하다.
- Promise와는 다르게 catch를 쓰지 않고 try catch block을 쓴다.
- async function의 반환값은 암묵적으로 Promise.resolve로 감싸진다.


### CRUD Operations Using Mongoose

#### Comparison Query Operators
- eq, ne, gt, gte, lt, lte, in, nin
- 오브젝트 안에서 비교를 표현하기 위해서 아래와 같이 처리한다.
```
const courses = await Course
  .find({ price: { $gt: 10 } })
```

#### Validation
- validate()는 void promise를 리턴한다. boolean을 주면 이상적일텐데 디자인 결함인 듯.
- required는 Mongoose에서만 의미있는 처리이다. MongoDB는 Mysql과 같이 validation을 DB level에서 신경쓰지 않는다.

#### Built-in Validators
- arrow function은 사용할 수 없는 이유 : arrow function은 자신만의 this가 없기 때문에 해당 function을 호출하는
mongoose의 function의 context를 따라갈 것이다.
```
price: {
    type: Number,
    required: function() { return this.isPublished; }
  }
```

#### Custom Validators
- empty array가 기본적으로 설정되기 때문에 tags를 설정하지 않으면 길이가 0으로 나온다.
- null일 경우는 체크가 되지 않기 때문에 아래와 같은 조건식으로 표현해줘야 한다.
```
tags: {
    type: Array,
    validate: {
      validator: function(v) {
        return v && v.length > 0;
      },
      message: 'A course should have at least on tag'
    }
  }
```

#### Asynchronous Validators
- 비동기 처리에 대해서 validator가 필요할 때 async validator를 지원한다.
- isAsync:true, callback을 사용하는 방식은 deprecated.
- 현재는 Promise가 권장되는 듯 하다.
```
validate: {
      isAsync: true,
      validator: function(v, callback) {
        setTimeout(() => {
          const result = v && v.length > 0;
          callback(result);
        }, 4000);
      },
      message: 'A course should have at least on tag'
    }

// Promise 사용
validate: {
      validator: function(v) {
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            const result = v && v.length > 0;
            resolve(result);
          }, 4000);
        })
      },
      message: 'A course should have at least on tag'
    }
```

#### SchemaType Options
- String: lowercase, uppercase, trim
- get, set은 로직이 실행되는 시점에 적용된다.
```
price: {
    type: Number,
    required: function() { return this.isPublished; },
    min: 10,
    max: 200,
    get: v => Math.round(v),
    set: v => Math.round(v)
  }

// db에 저장된 값이 15.8이라도 로직 실행 후엔 16으로 표현됨.
```

#### Error handling middleware
- 강의엔 나오지 않지만 MongoDB 규칙에 맞는 24자리 ID가 아닌 경우 예외처리는 router 미들웨에서 처리한다.
- 이것 때문에 개고생했다. 강의에선 이러한 예외에 대해선 언급이 없었다.
```
router.use('/:id', (req, res, next) => {
  if (!mongoose.Types.ObjectId.isValid(req.params.id)) {
    return res.status(404).send('Invalid ID.');
  }

  next();
});
```
