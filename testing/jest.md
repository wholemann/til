### toBe vs toEqual

- toEqual() : 객체의 속성과 값이 동일한지, 배열의 요소가 동일한지 비교할 때 쓰임.

- toBe() : === 에서 `Object.is` 로 변경됨.

- Object.is는 기본적으로 ===과 비슷하나 아래와 같은 케이스에서 다름.
```
NaN === NaN is false, but Object.is(NaN, NaN) is true
+0 === -0 is true, but Object.is(+0, -0) is false
-0 === +0 is true, but Object.is(-0, +0) is false
```

### toThrow()
- You must wrap the code in a function, otherwise the error will not be caught and the assertion will fail.
```
test('throws on octopus', () => {
  expect(() => {
    drinkFlavor('octopus');
  }).toThrow();
});
```

### Configuring Jest
- Jest는 json 파일로 변경을 감지하여 --watchAll 모드일 때 테스트를 re-run 해버린다.
- 무시하고 싶은 파일이나 디렉토리가 있다면 아래와 같이 package.json에 작성해야한다.

```
// package.json

jest: {
  "watchPathIgnorePatterns": [ "/path/file" ]
}
```
