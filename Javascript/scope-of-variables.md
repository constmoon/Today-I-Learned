# 변수의 Scope(범위)
프로그램에서 사용되는 변수들은 사용 가능한 범위를 가진다. 그 범위를 변수의 스코프(scope)라고 한다.


## ES5


기존 자바스크립트가 사용하는 `var` 는 `{}` 에 상관없이 함수 단위로 스코프가 설정된다. 이를 `function-scoped` 라 한다. 
```
function counter () {
    for(var i=0; i<10; i++) {
        console.log(i)
    }
}
counter()
console.log('after loop i is', i) // ReferenceError: i is not defined
```
```
for(var i=0; i<10; i++) {
    console.log(i)
}
console.log('after loop i is ', i) // after loop i is 10
```

위의 경우에는 에러가 발생하지만, 아래의 경우엔 for문에서 사용했던 i를 그대로 출력할 수 있다. 이건 var가 호이스팅(Hoisting) 되었기 때문이다.

## Hoisting
Hoisting이란 단어를 직역하면 끌어올리기, 들어올려 나르기라는 뜻이다. 자바스크립트에서의 호이스팅 또한 마찬가지로 변수를 끌어올린다는 의미이다.

`var` 로 선언된 모든 변수 선언은 호이스팅 된다. 이는 변수의 정의가 그 범위에 따라 **선언**과 **할당**으로 분리되는 것을 의미한다. 즉, 변수가 함수 내에서 정의되었을 경우, 선언이 함수의 최상위로, 함수 바깥에서 정의되었을 경우, 전역 컨텍스트의 최상위로 변경이 된다.

```
function getX() {
    console.log(x); // undefined
    var x = 100;
    console.log(x); // 100
}
getX();
```

변수 x를 선언하지 않고 출력하려 한다면 오류가 나는 것이 일반적이지만, 자바스크립트에서는 undefined라고 처리되며 넘어간다. `var x = 100` 에서 `var x` 를 호이스트하기 때문이다. 작동 순서에 맞게 코드를 재구성하면 다음과 같다.
```
function getX() {
    var x;
    console.log(x);
    x = 100;
    console.log(x);
}
getX();
```

## ES6
ES6는 블록 단위 `{}` 로 변수의 범위가 제한된다. 이를 `block-scoped` 라고 한다.
```
let sum = 0; 
for(let i=1; i<=5; i++){
    sum = sum + i; 
}
console.log(sum);  // 15
console.log(i);    // Uncaught Reference Error: i is not defined
```

## const & let
기존 ES5의 `var` 는 다음과 같은 문제점이 있었다.

```
// 이미 선언한 변수 이름으로 재선언했는데 아무런 문제가 발생하지 않는다.
var a = '10'
var a = '20'

// hoisting으로 인해 Reference 에러가 나지 않는다.
c = 'test'
var c
```
ES6에 추가된 `const` 와  `let` 는 변수 재선언이 불가능하다.


### const
`const` 로 선언한 값은 변경할 수 없다.

```
const a = 10;
a = 20; // Uncaught TypeError: Assignment to constant variable
```
하지만 객체나 배열의 내부는 변경할 수 있다.
```
const a = {};
a.num = 10; 
console.log(a);    // {num: 10}
const a = []; 
a.push(20); 
console.log(a);    // [20]
```
### let
`let` 로 선언한 값에 대해서 다시 선언할 수 없다. 그러나 한 번 선언한 값에 대해서 값은 변경할 수 있다.
```
let b = 10;
let b = 20;    // Uncaught SyntaxError: Identifier 'b' has already been declared
b = 40;
console.log(b);    // 40
```  

`let`, `const`가 호이스팅이 발생하지 않는건 아니다. `var`가 `function-scoped`로 호이스팅 되었다면  `let`, `const`는 `block-scoped` 단위로 호이스팅이 일어나는데... 이와 관련해서는 Temporal Dead Zone(TDZ)을 공부하면서 같이 보는 걸로.  

#
참고
>   
>   [https://asfirstalways.tistory.com/197](https://asfirstalways.tistory.com/197)
>   [https://gist.github.com/LeoHeo/7c2a2a6dbcf80becaaa1e61e90091e5d](https://gist.github.com/LeoHeo/7c2a2a6dbcf80becaaa1e61e90091e5d)
