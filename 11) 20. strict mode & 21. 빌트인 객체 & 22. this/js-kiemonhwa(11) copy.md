20 strict mode & 21 빌트인 객체 & 22. this



# 20장 strict mode

## 1. Strict mode 란?

자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 **문제를 일으킬 수 있는 코드에 대한 명시적인 에러를 발생**시킨다.

```js
function foo() {
  x = 10;
}
foo();

console.log(x); // ?
```

위 예제를 실행하면 결과는 `10`이 출력된다.
`x = 10` 코드를 만나면 자바스크립트 엔진은 `foo`함수 스코프에서 `x`식별자를 검색한다.
`foo`함수 스코프에 없으면 스코프 체인을 통해 상위 스코프에서 `x` 변수를 검색한다. 즉, 전역 스코프에서 검색하는데, 전역 스코프에 없으면 `RefferenceError`가 발생해야 하는데 자바스크립트 엔진은 암묵적으로 전역 객체에 `x`프로퍼티를 동적으로 생성한다. 이를 **암묵적 전역**`implicit global`이라 한다.

이런 현상은 개발자의 의도와 다르게 동작할 우려가 있어 좋지 않다.
따라서 반드시 `var`, `let`, `const`키워드를 사용하여 변수를 선언한 다음 사용해야 한다. (키워드만 적어줘도 암묵적 전역은 발생하지 않는다.)



## 2. Strict mode 의 적용

`strict mode`를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 `'use strict';`를 추가한다. 전역의 선두에 추가하면 스크립트 전체에 `strict mode`가 적용된다.

전역 선두에 선언

```
'use strict';

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

함수 몸체 선두에 선언

```js
function foo() {
  'use strict';

  x = 10; // ReferenceError: x is not defined
}
foo();
```

`'use strict';`를 선두에 위치하지 않으면 제대로 동작하지 않는다.

```js
function foo() {
  x = 10; // 에러를 발생시키지 않는다.
  'use strict';
}
foo();
```



## 3. 전역에 strict mode 를 적용하는 것은 피하자

전역에 적용한 `strict mode`는 스크립트 단위로 적용된다.

```js
<!DOCTYPE html>
<html>
<body>
  <script>
    'use strict';
  </script>
  <script>
    x = 1; // 에러가 발생하지 않는다.
    console.log(x); // 1
  </script>
  <script>
    'use strict';

    y = 1; // ReferenceError: y is not defined
    console.log(y);
  </script>
</body>
</html>
```

위와 같이 스크립트 단위로 적용된 `strict mode`는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.

`strict mode` 와 `non-strict mode`를 혼용해서 사용하면 오류를 발생시킬 수 있다. 특히 외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 `non-strict mode`인 경우도 있기 때문에 전역에 `strict mode`를 적용하는 것은 바람직하지 않다.

즉시 실행 함수의 선두에 `strict mode`를 적용한다.

```js
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  'use strict';

  // Do something...
}());
```



## 4. 함수 단위로 strict mode 를 적용하는 것도 피하자

함수 단위로 `strict mode`를 적용하게되면 모든 함수마다 일일이 `strict mode`를 적용해야 하는데 이는 매우 번거로운 일이다.

그리고 `strict mode`가 적용된 함수가 참조할 함수 외부의 컨텍스트에 `strict mode`를 적용하지 않는다면 문제가 발생할 수 있다.

```js
(function () {
  // non-strict mode
  var lеt = 10; // 에러가 발생하지 않는다.

  function foo() {
    'use strict';

    let = 20; // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
}());
```



## 5. strict mode 가 발생시키는 에러

`strict mode`를 적용시켰을 때 발생하는 에러는 다음과 같다.

- 암묵적 전역
- 변수, 함수, 매개변수의 삭제(`delete`)
- 매개변수 이름의 중복
- with문의 사용



### 5.1 암묵적 전역

선언하지 않은 변수를 참조하면 `ReferenceError`가 발생한다.

```js
(function () {
  'use strict';

  x = 1;
  console.log(x); // ReferenceError: x is not defined
}());
```



### 5.2 변수, 함수, 매개변수의 삭제

`delete`연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError`가 발생한다.

```js
(function () {
  'use strict';

  var x = 1;
  delete x;
  // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a;
    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo;
  // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```



### 5.3 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 `SyntaxError`가 발생한다.

```js
(function () {
  'use strict';

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```



### 5.4 with문의 사용

`with`문을 사용하면 `SyntaxError`가 발생한다.

`with`문은 전달된 객체를 스코프 체인에 추가한다. `with`문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠져 사용하지 않는것이 좋다.

```js
(function () {
  'use strict';

  // SyntaxError: Strict mode code may not include a with statement
  with({ x: 1 }) {
    console.log(x);
  }
}());
```



## 6. Strict mode 적용에 의한 변화



### 6.1 일반 함수의 this

`strict mode`에서 함수를 일반함수로 호출하면 `this`에 `undefined`가 바인딩된다.

생성자 함수가 아닌 일반 함수 내부에서는 `this`를 사용할 필요가 없기 때문이다.

```js
(function () {
  'use strict';

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
}());
```

만약 `strict mode`가 아닌 경우 `window`전역 객체가 바인딩된다.



### 6.2 arguments 객체

`strict mode`에서 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```js
(function (a) {
  'use strict';
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
}(1));
```



---



# 21장 빌트인 객체



## 1. 자바스크립트 객체의 분류

- **표준 빌트인 객체(Standard built-in objects)**
  표준 빌트인 객체는 ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통 기능을 제공한다. `Object`, `String`, `Number`, `Boolean`, `Symbol`, `Date`, `Math`, `RegExp`, `Array`, `Map/Set`, `WeakMap`, `WeakSet`, `Function`, `Promise`, `Reflect`, `Proxy`, `JSON`, `Error`등 40여개 객체를 제공한다.
- **호스트 객체(host objects)**
  호스트 객체는 ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경(브라우저 환경 또는 Node.js환경)에서 추가로 제공하는 객체를 말한다.
  브라우저 환경에서는 `DOM`, `BOM`, `Canvas`, `XMLHttpRequest`, `fetch`, `requestAnimationFrame`, `SVG`, `Web Storage`, `Web Component`, `Web Worker`와 같은 [클라이언트 사이드 Web API](https://developer.mozilla.org/ko/docs/Web/API)를 호스트 객체로 제공하고,
  Node.js환경에서는 [Node.js고유의 API](https://nodejs.org/api/)를 호스트 객체로 제공한다.
- **사용자 정의 객체(user-defined objects)**
  사용자 정의 객체는 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 **사용자가 직접 정의한 객체**를 말한다.



## 2. 표준 빌트인 객체

```
Object`, `String`, `Number`, `Boolean`, `Symbol`, `Date`, `Math`, `RegExp`, `Array`, `Map/Set`, `WeakMap`, `WeakSet`, `Function`, `Promise`, `Reflect`, `Proxy`, `JSON`, `Error
```

`Math`, `Reflect`, `JSON`을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드(`Object.prototype.toString`)와 정적 메서드(`Object.create`등)를 제공하고,
생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.



`String`, `Number`, `Boolean`, `Function`, `Array`, `Date`는 생성자 함수로 호출하여 인스턴스를 생성할 수 있다.



```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}
console.log(typeof strObj);       // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj);     // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true); // Boolean {true}
console.log(typeof boolObj);      // object

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x'); // ƒ anonymous(x )
console.log(typeof func);                       // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr);        // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp);         // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();  // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
console.log(typeof date); // object
```

생성자 함수인 표준 빌트인 객체로 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

인스턴스는 프로토타입 메서드를 사용할 수 있다.

`Number` 빌트인 객체는 인스턴스 없이 정적으로 호출할 수 있는 정적 메서드도 제공한다.

```js
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```



## 3. 원시값과 래퍼 객체

문자열, 숫자, 불리언 등의 원시값이 있는데도 `String`, `Number`, `Boolean`등의 표준 빌트인 생성자 함수가 존재하는 이유가 뭘까?

```
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

위 예제와 같이 `str`은 원시값인 문자열을 담은 변수인데, `str.length`, `str.toUpperCase()`와 같이 프로퍼티와 메서드를 호출할 수 있다.

이는 원시값에 객체처럼 **마침표 표기법**으로 접근하면 자바스크립트 엔진이 **일시적으로 원시값을 연관된 객체로 변환**해 주기 때문이다.

즉, 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

이러한 임시 객체를 래퍼 객체(`wrapper object`)라고 한다.

```js
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

래퍼 객체가 생성되고 사용된 후 객체의 처리가 종료되면 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된 원시값으로 원래의 상태로(원시 값)되돌리고,
래퍼 객체는 가비지 컬렉션의 대상이 되어 사라진다.

숫자 값도 마찬가지다. 래퍼 객체의 `[[NumberData]]`내부 슬롯에 할당되고, 처리가 종료되면 원시값으로 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

```js
const num = 1.5;

// 원시 타입인 숫자가 래퍼 객체인 Number 객체로 변환된다.
console.log(num.toFixed()); // 2

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof num, num); // number 1.5
```

`Symbol`함수도 마찬가지로 래퍼 객체를 생성한다.

※ `null`, `undefined`는 래퍼 객체를 생성하지 않는다. 따라서 `null`과 `undefined`값을 객체처럼 사용하면 에러가 발생한다.



## 4. 전역 객체

전역 객체(`global object`)는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체다.

브라우저 환경에서는 `window`, Node.js 환경에서는 `global`이다.



전역 객체의 프로퍼티

- 표준 빌트인 객체(`Object`, `String`, `Number`, `Function`, `Array`등)
- 환경에 따른 호스트 객체(클라이언트 Web API, Node.js의 호스트 API)
- `var`키워드로 선언한 전역 변수
- 전역 함수



전역 객체의 특징

- 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- 전역 객체의 프로퍼티를 참조할 때 `window`(또는 `global`)를 생략할 수 있다.

```js
window.parseInt === parseInt; // -> true
```

- 전역 객체는 `Object`, `String`, `Number`, `Boolean`등 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행환경(브라우저 또는 Node.js)에 따라 추가적인 프로퍼티와 메서드를 갖는다.(브라우저: DOM, BOM, Canvas, XMLHttpRequest등 / Node.js: Node.js 고유의 API)
- `var`키워드로 선언한 전역 변수, 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.

```js
// var 키워드로 선언한 전역 변수
var foo = 1;
console.log(window.foo); // 1

// 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티다.
bar = 2; // window.bar = 2
console.log(window.bar); // 2

// 전역 함수
function baz() { return 3; }
console.log(window.baz()); // 3
```

- `let`이나 `const`키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

```js
let foo = 123;
console.log(window.foo); // undefined
```

- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 `window`를 공유한다.




### 4.1 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다.



**Infinity**
`Infinity`프로퍼티는 무한대를 나타내는 숫자값 `Infinity`를 갖는다.

```js
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3/0);  // Infinity
// 음의 무한대
console.log(-3/0); // -Infinity
// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```



**NaN**
`NaN`프로퍼티는 숫자가 아님(`Not a Number`)을 나타내는 숫자값 `NaN`을 갖는다.
`NaN`프로퍼티는 `Number.NaN`프로퍼티와 같다.

```js
console.log(window.NaN); // NaN

console.log(Number('xyz')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // number
```



**undefined**
`undefined`프로퍼티는 원시 타입 `undefined`를 값으로 갖는다.

```js
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```



### 4.2 빌트인 전역 함수

빌트인 전역 함수는 애플리케이션 전역에서 호추할 수 있는 빌트인 함수로서 전역 객체의 메서드다.



**eval**
`eval`함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받아, 전달받은 문자열 코드가 표현식 이라면 `eval`함수는 문자열 코드를 런타임에 평가하여 값을 생성하고,
전달받은 인수가 표현식이 아닌 문이라면 `eval`함수는 문자열 코드를 런타임에 실행한다.

```js
/**
 * 주어진 문자열 코드를 런타임에 평가 또는 실행한다.
 * @param {string} code - 코드를 나타내는 문자열
 * @return {*} 문자열 코드를 평가/실행한 결과값
 */
eval(code)
// 표현식인 문
eval('1 + 2;'); // -> 3

// 표현식이 아닌 문
eval('var x = 5;'); // -> undefined

// eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언되었다.
console.log(x); // 5

// 객체 리터럴은 반드시 괄호로 둘러싼다.
const o = eval('({ a: 1 })');
console.log(o); // {a: 1}

// 함수 리터럴은 반드시 괄호로 둘러싼다.
const f = eval('(function() { return 1; })');
console.log(f()); // 1
```

여러개의 문으로 이루어져 있다면 모든 문을 실행한 다음, 마지막 결과값을 반환한다.

```js
console.log(eval('1 + 2; 3 + 4;')); // 7
```

`eval`함수는 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정한다.

```js
const x = 1;

function foo() {
  // eval 함수는 런타임에 foo 함수의 스코프를 동적으로 수정한다.
  eval('var x = 2;');
  console.log(x); // 2
}

foo();
console.log(x); // 1
```

`strict mode`에서는 `eval`함수는 기존의 스코프를 수정하지 않고 `eval`함수 자신의 자체적인 스코프를 생성한다.

```js
const x = 1;

function foo() {
  'use strict';

  // strict mode에서 eval 함수는 기존의 스코프를 수정하지 않고 eval 함수 자신의 자체적인 스코프를 생성한다.
  eval('var x = 2; console.log(x);'); // 2
  console.log(x); // 1
}

foo();
console.log(x); // 1
```

인수로 전달받은 문자열 코드가 `let`, `const` 키워드를 사요한 변수 선언문이라면 암묵적으로 `strict mode`가 적용된다.

```js
const x = 1;

function foo() {
  eval('var x = 2; console.log(x);'); // 2
  // let, const 키워드를 사용한 변수 선언문은 strict mode가 적용된다.
  eval('const x = 3; console.log(x);'); // 3
  console.log(x); // 2
}

foo();
console.log(x); // 1
```

※ `eval`함수는 보안에 매우 취약하다.
또한, 최적화가 수행되지 않아 일반적인 코드 실행에 비해 느리다.
따라서 `eval`**함수의 사용은 금지해야 한다.**



**isFinite**
전달받은 인수가 정상적인 유한수 인지 검사하여 유한수이면 `true`, 무한수이면 `false`를 반환한다.

```js
// 인수가 유한수이면 true를 반환한다.
isFinite(0);    // -> true
isFinite(2e64); // -> true
isFinite('10'); // -> true: '10' → 10(문자열->숫자)
isFinite(null); // -> true: null → 0(null->0)

// 인수가 무한수 또는 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(Infinity);  // -> false
isFinite(-Infinity); // -> false

// 인수가 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(NaN);     // -> false
isFinite('Hello'); // -> false
isFinite('2005/12/12'); // -> false
```



**isNaN**
전달받은 인수가 `NaN`인지 검사하여 그 결과를 불리언 타입으로 반환한다. 전달받은 인수의 타입이 숫자가 아닌경우 숫자로 타입을 변환한 후 검사를 수행한다.

```js
// 숫자
isNaN(NaN); // -> true
isNaN(10);  // -> false

// 문자열
isNaN('blabla'); // -> true: 'blabla' => NaN
isNaN('10');     // -> false: '10' => 10
isNaN('10.12');  // -> false: '10.12' => 10.12
isNaN('');       // -> false: '' => 0
isNaN(' ');      // -> false: ' ' => 0

// 불리언
isNaN(true); // -> false: true → 1
isNaN(null); // -> false: null → 0

// undefined
isNaN(undefined); // -> true: undefined => NaN

// 객체
isNaN({}); // -> true: {} => NaN

// date
isNaN(new Date());            // -> false: new Date() => Number
isNaN(new Date().toString()); // -> true:  String => NaN
```



**parseFloat**
전달받은 문자열 인수를 부동 소수점 숫자(`floating point number`)즉, 실수로 해석하여 반환한다.

```js
// 문자열을 실수로 해석하여 반환한다.
parseFloat('3.14');  // -> 3.14
parseFloat('10.00'); // -> 10

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat('34 45 66'); // -> 34
parseFloat('40 years'); // -> 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat('He was 40'); // -> NaN

// 앞뒤 공백은 무시된다.
parseFloat(' 60 '); // -> 60
```



**parseInt**
전달받은 문자열 인수를 정수(`integer`)로 해석하여 반환한다.
두 번째 인수로 진법을 나타내는 기수(2~36)을 전달할 수 있다.

```js
// 문자열을 정수로 해석하여 반환한다.
parseInt('10');     // -> 10
parseInt('10.123'); // -> 10

parseInt(10);     // -> 10
parseInt(10.123); // -> 10

// 10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10'); // -> 10
// '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 2); // -> 2
// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 8); // -> 8
// '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 16); // -> 16
```

기수를 지정하여 10진수 숫자를 문자열로 변환하여 반환하려면 `Number.prototype.toString`메서드를 사용한다.

```js
const x = 15;

// 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환한다.
x.toString(2); // -> '1111'
// 문자열 '1111'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString(2), 2); // -> 15

// 10진수 15를 8진수로 변환하여 그 결과를 문자열로 반환한다.
x.toString(8); // -> '17'
// 문자열 '17'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString(8), 8); // -> 15

// 10진수 15를 16진수로 변환하여 그 결과를 문자열로 반환한다.
x.toString(16); // -> 'f'
// 문자열 'f'를 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString(8), 8); // -> 15

// 숫자값을 문자열로 변환한다.
x.toString(); // -> '15'
// 문자열 '15'를 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString()); // -> 15
```

문자열이 16진수 리터럴(`0x`또는 `0X`로 시작)이면 16진수로 해석하여 10진수 정수로 반환한다.

```js
// 16진수 리터럴 '0xf'를 16진수로 해석하고 10진수 정수로 그 결과를 반환한다.
parseInt('0xf'); // -> 15
// 위 코드와 같다.
parseInt('f', 16); // -> 15
```

2진수, 8진수 리터럴은 안된다.
0으로 시작하는 숫자로 인식해 버린다.

```js
// 2진수 리터럴(0b로 시작)은 제대로 해석하지 못한다. 0 이후가 무시된다.
parseInt('0b10'); // -> 0
// 8진수 리터럴(ES6에서 도입. 0o로 시작)은 제대로 해석하지 못한다. 0 이후가 무시된다.
parseInt('0o10'); // -> 0
```

두 번째 인수로 명확하게 알려줘야 한다.

```js
// 문자열 '10'을 2진수로 해석한다.
parseInt('10', 2); // -> 2
// 문자열 '10'을 8진수로 해석한다.
parseInt('10', 8); // -> 8
```

아래 참고

```js
// 'A'는 10진수로 해석할 수 없다.
parseInt('A0'); // -> NaN
// '2'는 2진수로 해석할 수 없다.
parseInt('20', 2); // -> NaN

// 10진수로 해석할 수 없는 'A' 이후의 문자는 모두 무시된다.
parseInt('1A0'); // -> 1
// 2진수로 해석할 수 없는 '2' 이후의 문자는 모두 무시된다.
parseInt('102', 2); // -> 2
// 8진수로 해석할 수 없는 '8' 이후의 문자는 모두 무시된다.
parseInt('58', 8); // -> 5
// 16진수로 해석할 수 없는 'G' 이후의 문자는 모두 무시된다.
parseInt('FG', 16); // -> 15

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseInt('34 45 66'); // -> 34
parseInt('40 years'); // -> 40
// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseInt('He was 40'); // -> NaN
// 앞뒤 공백은 무시된다.
parseInt(' 60 '); // -> 60
```



**encodeURI / decodeURI**
`encodeURI`함수는 완전한 `URI Uniform Resource Identifier`를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.
`URI`는 인터넷에 있는 자원을 나타내는 유일한 주소를 말한다.

> ※ *이스케이프 처리*
> 이스케이프 처리란 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 **아스키 문자 셋**으로 변환하는 것이다. 예를들면 한글’가’는 %EC%9E%90 으로 인코딩된다.

```js
// 완전한 URI
const uri = 'http://example.com?name=이웅모&job=programmer&teacher';

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
```

`decodeURI`함수는 인코딩된 `URI`를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

```js
const uri = 'http://example.com?name=이웅모&job=programmer&teacher';

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

// decodeURI 함수는 인코딩된 완전한 URI를 전달받아 이스케이프 처리 이전으로 디코딩한다.
const dec = decodeURI(enc);
console.log(dec);
// http://example.com?name=이웅모&job=programmer&teacher
```



**encodeURIComponent / decodeURIComponent**
`encodeURIComponent`함수는 `URI`구성요소를 인수로 전달받아 인코딩한다.(이스케이프 처리)
`decodeURIComponent`함수는 매개변수로 전달된 `URI`구성요소를 디코딩한다.

`encodeURIComponent`함수는 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다. 따라서 쿼리스트링 구분자로 사용되는 `=`, `?`, `&`까지 인코딩한다.

반면 `endcodeURI`함수는 매개변수로 전달된 문자열을 완전한 URI전체라고 간주한다. 따라서 `=`, `?`, `&`은 이코딩하지 않는다.

```js
// URI의 쿼리 스트링
const uriComp = 'name=이웅모&job=programmer&teacher';

// encodeURIComponent 함수는 인수로 전달받은 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다.
// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다.
let enc = encodeURIComponent(uriComp);
console.log(enc);
// name%3D%EC%9D%B4%EC%9B%85%EB%AA%A8%26job%3Dprogrammer%26teacher

let dec = decodeURIComponent(enc);
console.log(dec);
// 이웅모&job=programmer&teacher

// encodeURI 함수는 인수로 전달받은 문자열을 완전한 URI로 간주한다.
// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &를 인코딩하지 않는다.
enc = encodeURI(uriComp);
console.log(enc);
// name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

dec = decodeURI(enc);
console.log(dec);
// name=이웅모&job=programmer&teacher
```



### 4.3 암묵적 전역

선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티로 등록된다. 이를 암묵적 전역(`implicit global`)이라 한다.

```js
var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

`foo`함수 내부에 `y = 20;`이 실행되면 참조 에러가 발생하지 않고, 전역 변수처럼 사용된다.
전역 객체의 프로퍼티로 등록되어 참조되기 때문이다.

하지만 `y`는 변수 선언 없이 **단지 전역 객체의 프로퍼티로 추가되었을 뿐**이다.
`y`**는 변수가 아니다.** 따라서 **변수 호이스팅도 발생하지 않는다.**

```js
// 전역 변수 x는 호이스팅이 발생한다.
console.log(x); // undefined
// 전역 변수가 아니라 단지 전역 객체의 프로퍼티인 y는 호이스팅이 발생하지 않는다.
console.log(y); // ReferenceError: y is not defined

var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

또한 `delete`연산자로 삭제할수 있다. 변수가 아니기 때문에!!

전역 변수는 프로퍼티이지만 `delete`연산자로 삭제할 수 없다.

```js
var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
  console.log(x + y);
}

foo(); // 30

console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제되지 않는다.
delete y; // 프로퍼티는 삭제된다.

console.log(window.x); // 10
console.log(window.y); // undefined
```



---



# 22. this



## 1. this 키워드

**객체**는 상태(`state`)를 나타내는 **프로퍼티**와 동작(`behavior`)를 나타내는 **메서드**를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 하는데, 이때 `this`를 사용한다.

```js
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
    return 2 * circle.radius;
  }
};

console.log(circle.getDiameter()); // 10
```

`getDiameter`메서드 내에서 메서드 자신이 속한 객체를 가리키는 식별자 `circle`을 참조하고 있다.

실행되는 순서

1. 변수 호이스팅으로 `circle` 변수 생성(초기화는X)
2. `circle`변수에 할당되기 직전에 객체 리터럴을 평가
3. `circle`객체 생성
4. `circle.getDiameter();`즉, `getDiameter`메서드가 호출되어 함수 몸체가 실행된다.
5. `getDiameter`메서드가 실행될 때 `circle`식별자를 참조한다.

`circle.getDiameter();`호출하는 시점에는 이미 `circle`객체가 생성되어 있으므로 메서드 내부에서 `circle`식별자를 참조할 수 있다.

하지만 자신이 속한 객체를 재귀적으로 참조하는 방식은 바람직하지 않다.

생성자 함수 방식으로 인스턴스를 생성하는 경우를 보자.

```js
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  ????.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  return 2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
```

함수를 선언하는 시점에는 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.

생성자 함수로 객체를 생성하려면 생성자 함수를 먼저 정의한 후 `new`연산자와 함께 생성자 함수를 호출해야 한다. 즉, 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.

생성자 함수를 정의하는 시점에는 인스턴스를 가리키는 식별자를 알 수 없기 때문에 특수한 식별자를 사용한다. 바로 `this`이다.

`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(`self-referenceing variable`)다.

`this`는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다. 함수를 호출하면 `arguments`객체와 `this`가 암묵적으로 함수 내부에 전달된다.

`this`가 가리키는 값, 즉 `this`바인딩은 **함수 호출 방식에 의해 동적으로 결정**된다.

객체 리터럴에서 `this`사용하는 예제

```js
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
```

생성자 함수에서 `this`사용하는 예제

```js
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

`java`나 `c++`같은 클래스 기반 언어에서 `this`는 항상 클래스가 생성하는 인스턴스를 가리킨다.

자바스크립트의 `this`는 함수가 호출되는 방식에 따라 `this`바인딩될 값이 동적으로 결정된다.

```js
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

`this`는 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.

일반 함수 내부의 `this`는 `window`가 바인딩되고, `strict mode`에서는 `undefined`가 바인딩된다.(굳이 필요없기 때문에)

```js
(function(){
    'use strict';
    function print(){
      console.log(this);
    }
    print();  // undefined
}())
```

하지만 아래 처럼 객체의 프로퍼티로 함수를 할당하면 메서드가 되어 객체가 `this`에는 객체가 바인딩 된다.

```js
(function(){
    'use strict';
    function printThis(){
      console.log(this);
    }
    const obj = {};
    obj.print = printThis;
    obj.print(); // obj: {print: ƒ}
}())
```

 

## 2. 함수 호출 방식과 this 바인딩

`this`바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

> **※ 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.**
> 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 결정된다.
> `this`는 함수가 호출시에 결정된다.

> **※ 함수 객체가 생성되는 시점**
> 함수 선언문은 코드(함수 선언문을 포함한) 평가 단계에서 함수 객체 생성
> 함수 표현식은 코드(함수 표현식을 포함한) 실행 단계에서 함수 객체 생성

함수를 호출하는 다양한 방식

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- `Function.prototype.apply/call/bind`메서드에 의한 간접 호출

```js
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```



### 2.1 일반 함수 호출

일반 함수로 호출하는 경우 기본적으로 `this`에는 전역 객체가 바인딩된다.

```js
function foo() {
  console.log("foo's this: ", this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

`strict mode`에서는 `this`에 `undefined`가 바인딩된다.

```js
function foo() {
  'use strict';

  console.log("foo's this: ", this);  // undefined
  function bar() {
    console.log("bar's this: ", this); // undefined
  }
  bar();
}
foo();
```

메서드 내의 중첩 함수라도 일반 함수로 호출되면 함수 내부의 `this`에는 전역 객체가 바인딩된다.

```js
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this);  // {value: 100, foo: ƒ}
    console.log("foo's this.value: ", this.value); // 100

    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }

    // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
    bar();
  }
};

obj.foo();
```

콜백 함수도 일반 함수로 호출되면 `this`에 전역 객체가 바인딩된다.

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  }
};

obj.foo();
```

메서드 내부 중첩 함수나 콜백 함수에 `this`바인딩을 메서드의 `this`와 일치 시키기 위한 방법은 아래 3가지가 있다.

**1)**`that`**사용**

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  }
};

obj.foo();
```

**2)**`Function.prototype.apply/call/bind`**메서드 활용**

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(function () {
      console.log(this.value); // 100
    }.bind(this), 100);
  }
};

obj.foo();
```

**3)화살표 함수 활용**

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  }
};

obj.foo();
```



### 2.2 메서드 호출

메서드 내부의 `this`에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표`.` 연산자 앞에 기술한 객체가 바인딩된다.

주의할 것은 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 **메서드를 호출한 객체에 바인딩** 된다는 것이다.

```js
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

만약 `getName`메서드를 일반 변수에 할당하여 호출하면 일반 함수로 호출된다.

```js
const person = {
  name: 'Lee',
  getName() {
    return this.name;
  }
};

const anotherPerson = {
  name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```

프로토타입 메서드 내부에서 사용된 `this`도 일반 메서드와 동일하게 해당 메서드를 호출한 객체에 바인딩 된다.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // ① Lee

Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // ② Kim
```

①의 경우 `getName`메서드를 호출한 객체는 `me`이다. 따라서 `getName`메서드 내부의 `this`는 `me`를 가리키며 `this.name`값은 `Lee`다.

②의 경우 `getName`메서드를 호출한 객체는 `Person.prototype`이고, 객체이므로 직접 메서드를 호출할 수 있다. 따라서 `getName`메서드 내부의 `this`는 `Person.prototype`을 가리키며 `this.name`값은 `Kim`이다.



### 2.3 생성자 함수 호출

생성자 함수 내부의 `this`에는 생성자 함수가 **(미래에) 생성할 인스턴스**가 바인딩된다.

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

`new`연산자와 함께 호출하지 않으면 일반 함수로 호출된다.
`Circle`함수에 반환 값이 없으면 암묵적으로 `undefined`를 반환한다. [※ 17장 생성자 함수에 의한 객체 생성 참조](https://hong-p.github.io/javascript/javascript-deepdive-ch17/#23-생성자-함수의-인스턴스-생성-과정)

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다. 즉, 일반적인 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

일반 함수로 호출된 경우 `Circle`내부의 `this`는 전역 객체`window`를 가리키기 때문에 `this.radius`는 결국 `window.radius`가 되어 `console.log(radius);`를 실행하면 `window.radius`값이 리턴되어 출력되게된다.



### 2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

`apply`, `call`, `bind`메서드는 `Function.prototype`의 메서드다. 즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.



**1)**`Function.prototype.apply`, `Function.prototype.call` **메서드**
`apply`와 `call`메서드는 함수에 `this`로 사용할 객체를 전달하고 함수를 호출한다.

```js
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray])

/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, agr2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

위 예제는 특정 객체만 전달하고 인수 리스트는 전달하지 않는다.

`apply`와 `call` 메서드의 본질적인 기능은 동일하다. 다만 인수를 전달하는 방식만 다르다.
아래 예제를 통해 인수 전달을 살펴보자.

```js
function getThisBinding() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}
```

`apply`메서드는 함수의 인수를 **배열로 묶어 전달**하고,
`call`메서드는 함수의 인수를 **쉼표로 구분한 리스트 형식으로 전달**한다.

`apply`와 `call` 메서드의 대표적인 용도는 `arguments`객체와 같은 **유사 배열 객체에 배열 메서드를 사용하는 경우**이다.

`arguments`객체의 자세한 내용은 [18.2.1 절 arguments프로퍼티](https://hong-p.github.io/javascript/javascript-deepdive-ch18/#21-arguments-프로퍼티)를 참고하자.

`arguments`객체는 배열이 아니기 때문에(유사 배열) `Array.prototype.slice`같은 배열 메서드를 사용할 수 없으나, `apply`나 `call`메서드를 이용하면 가능하다.

```js
function convertArgsToArray() {
  console.log(arguments);

  // arguments 객체를 배열로 변환
  // Array.prototype.slice를 인수없이 호출하면 배열의 복사본을 생성한다.
  const arr = Array.prototype.slice.call(arguments);
  // const arr = Array.prototype.slice.apply(arguments);
  console.log(arr);

  return arr;
}

convertArgsToArray(1, 2, 3); // [1, 2, 3]
```

**2)** `Function.prototype.bind` **메서드**
`bind`메서드는 `apply`와 `call`메서드와 달리 함수를 호출하지는 않고 `this`로 사용할 객체만 전달한다.(명시적으로 호출`()`을 해줘야 함)

```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding

// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

`bind`함수는 메서드의 `this`와 메서드 내부의 중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```js
const person = {
  name: 'Lee',
  foo(callback) {
    // ①
    setTimeout(callback, 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // ② Hi! my name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
  // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
  // Node.js 환경에서 this.name은 undefined다.
});
```

`person.foo` 메서드가 호출되는 ①의 시점, 콜백 함수가 호출되기 직전의 `this`값은 `foo`메서드를 호출한 객체, 즉 `person`이다.

하지만 콜백 함수가 호출된 ②의 시점에서 `this`는 전역 객체 `window`를 가리킨다. 따라서 `person.foo`의 콜백 함수 내부에서 `this.name`은 `window.name`과 같다.

`person.foo`의 콜백 함수는 외부 함수 `person.foo`를 돕는 헬퍼 함수(보조 함수) 역할을 하기 때문에 외부 함수 `person.foo` 내부의 `this`와 콜백 함수 내부의 `this`가 상이하면 문맥상 문제가 된다.

이러한 경우 `bind`메서드를 사용해 이를 해결한다.

```js
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

혹은, 콜백 함수를 화살표 함수로 선언해 해결이 가능하다.
참고로 화살표 함수는 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않기 때문에 스코프 체인을 따라 상위 스코프의 `this`를 그대로 참조 한다. 이를 `lexical this`라고 한다.

아래와 같은 예제는 화살표 함수를 인수로 전달하기 때문에 `person.foo`메서드가 호출되면, `foo`함수 객체가 생성되고, 그에 따라 매개변수(`callback`)도 평가되고 화살표 함수 객체가 생성된다.(**이때 렉시컬 스코프가 결정된다.**)

따라서 화살표 함수(`callback`)의 상위 스코프는 `person.foo`함수가 되어, 화살표 함수의 `this`는 `person.foo`함수에 바인딩된 `this`가 되고, 이`this`는 `person`을 가리키게 된다.

```js
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback, 100);
  }
};

person.foo(() => {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

