# Binary & Unary Operators

## Unary operator 순서의 중요성

**using after** - you are going to receive the current value of the operand before the increment has been executed

```javascript
x++
````

**using before** - you will increment the value of the operand first and then receive the new value

```javascript
++x
```

예시:
```javascript
let x = 5;
console.log(x++); // 5 <- 값이 반환된 다음에 increment 되었기 때문에 5.
console.log(x); // 4 <- 이제야 값이 increment 되었기 때문에 4.

let y = 10;
console.log(--y); // 9 값이 반환되기 전에 decrement 되었기 떄문에 9.
console.log(y); // 9 값은 똑같이 되있다.
```

## 문자열을 숫자로 바꿔야 할때

이건 진짜 간단하다. 그냥 `+` 붙여주면 된다

```javascript
let x = '10';
let y = '5';
let z = '-2';

console.log(+x); // 10
console.log(typeof +x); // number

console.log(-y); // -5
console.log(typeof -y); // number

console.log(-z); // 2; <- turns into a positive number
console.log(typeof z); // number
```

**또한 boolean 값을 integer(정수?)로 바꿀수도 있다.**

```javascript
let a = true;
let b = false;

console.log(+a); // 1
console.log(-b); // 0
console.log(-a); // -1
console.log(+b); // 0
```

