# What is TypeScript?

## TypeScript
> Is a programming language, but also a tool. It's a powerful compiler which you run over your code to compile your TypeScript code to JavaScript.
> It adds "types". It adds features to JavaScript language which gives the developer an opportunity of identifying errors in the code eariler before the script runs and the error occurs at run time in the browser.

## Example of TS when it's useful

```
const button = document.querySelector("button");
const input1 = document.getElementById("num1");
const input2 = document.getElementById("num2");

function add(num1, num2) {
  return num1 + num2;
}

button.addEventListener("click", function () {
  console.log(add(input1.value, input2.value));
});
```

여기서 유저가 input field에 `2`와 `3`를 적을 경우, 콘솔에 찍히는 값은 `5`가 아닌 `23`이 찍히게된다. 무조건 들어오는 값은 number이 아닌 string으로 들어오기 떄문이다. 물론 JS로 validation같은 컨디셔널을 넣거나 해서 할수 있지만 TS에서 제공하는 툴로 에러를 최소화 할 수 있다.

**Example:**

```
function add(num1, num2) {
   if (typeof num1 === 'number' && typeof num2 === 'number'){
      return num1 + num2;
   } else {
      return +num1 + +num2;
   }
}

button.addEventListener("click", function () {
  console.log(add(input1.value, input2.value));
});
```

이렇게 하게되면 유저가 `5`와 `10` 을 적는다는 가정하에 `15`가 찍히게 된다.
그런데 이건 너무 **귀찮아** 지고 에러를 잡기 위해서 extra code를 쓰게 된다.


