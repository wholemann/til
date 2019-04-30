## EventListener

resize 이벤트가 발생했을 때 함수가 작동하길 원한다면 addEventListener 인자에 handleResize()가 아닌 handleResize로 넣어줘야 한다.

```
function handleResize() {
    console.log("I have been resized");
}

window.addEventListener("resize", handleResize);
```

모든 Event Name
https://developer.mozilla.org/en-US/docs/Web/Events

---

### Array.prototypejoin()
- 배열의 모든 요소를 연결해 하나의 문자열로 만들어준다.
- () 안의 구분자로 연결한다. 기본은 ',' 쉼표로 연결한다.
```
var elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"

console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"
```
---

### Different result types of `+` by operand types
```
            || undefined | null   | boolean | number | string | object |
=========================================================================
 undefined  || number    | number | number  | number | string | string | 
 null       || number    | number | number  | number | string | string | 
 boolean    || number    | number | number  | number | string | string | 
 number     || number    | number | number  | number | string | string | 
 string     || string    | string | string  | string | string | string | 
 object     || string    | string | string  | string | string | string | 
 ```
 * applies to Chrome13, FF6, Opera11 and IE9. Checking other browsers and versions is left as an exercise for the reader.

 https://stackoverflow.com/a/7124907