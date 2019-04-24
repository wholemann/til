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

