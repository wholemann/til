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

#### Handling HTTP Delete Requests
