
### Array.from()

```
// Using an arrow function as the map function to
// manipulate the elements
Array.from([1, 2, 3], x => x + x);      
// [2, 4, 6]

// Generate a sequence of numbers
// Since the array is initialized with `undefined` on each position,
// the value of `v` below will be `undefined`
Array.from({length: 5}, (v, i) => i);
// [0, 1, 2, 3, 4]
```


https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Array/from#Einsatz_von_Arrow-Funktionen_und_Array.from