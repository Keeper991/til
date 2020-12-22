# 피연산자가 boolean이 아닐 때, 논리 연산자가 동작하는 방법

###### update History

2020.12.23 - 최초 작성.





## OR

**(여기에 나오는 true와 false는 boolean이 아닌 값을 의미함.)**

```javascript
// true || false
const a = true;
const b = false;
const ret = a || b; 	// ret = a;

// false || true
const a = false;
const b = true;
const ret = a || b; 	// ret = b;

// false || false
const a = false;
const b = false;
const ret = a || b; 	// ret = b;

// true || true
const a = true;
const b = true;
const ret = a || b; 	// ret = a;
```



## AND

```javascript
// true && false
const a = true;
const b = false;
const ret = a && b; 	// ret = b;

// false && true
const a = false;
const b = true;
const ret = a && b; 	// ret = a;

// false && false
const a = false;
const b = false;
const ret = a && b; 	// ret = a;

// true && true
const a = true;
const b = true;
const ret = a && b; 	// ret = b;
```



## 결론

논리를 결정짓는 변수의 값을 리턴한다고 생각하면 된다.

AND의 경우,
리턴 값이 true이면 둘 다 true여야하므로, 우측 피연산자.
리턴 값이 false이면 false로 인해 false가 리턴되므로 false인 피연산자.

OR의 경우,

리턴 값이 true이면 true로 인해 true가 리턴되므로 먼저 true로 입력된 피연산자(둘 다 true일 경우, 좌측 피연산자).
리턴 값이 false이면 둘 다 false여야하므로, 우측 피연산자.