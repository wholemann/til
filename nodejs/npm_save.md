#### --save
```
$ npm install --save jest
```
만약 현재 경로에 `package.json` 이 존재하면 아래와 같이 `dependencies` 항목에 자동으로 포함된다.

```
"dependencies": {
    "jest": "^24.7.1"
  }
```

#### --save-dev
```
$ npm install --save-dev jest
```
만약 현재 경로에 `package.json` 이 존재하면 아래와 같이 `devDependencies` 항목에 자동으로 포함된다.
```
"devDependencies": {
    "jest": "^24.7.1"
  }
```

#### 언제쓸까?
다른 곳에서 해당 프로젝트를 실행해야 할 경우 `$ npm install`만 입력하면 dependencies 항목에 있는 모듈들을 자동으로 설치한다.
단, dependencies, devDependencies 에 있는 모든 항목을 설치하므로 보통 다음과 같이 적용한다.
```
// dependencies만 설치(production 용)
$ npm install --only=prod

// devDependencies만 설치(development 용)
$ npm install --only=dev
```