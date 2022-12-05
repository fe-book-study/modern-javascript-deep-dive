7) 14) 전역변수의 문제점 & 15) let, const의 블록레벨스코프 



# 14강 전역변수의 문제점



## 1. 변수의 생명 주기

### 1.1 지역 변수의 생명 주기

> 변수는 선언에 의해 생성되고 할당을 통해 값을 갖는다. 그리고 언젠가 소멸한다.
>
> 즉, 변수는 생성되고 소멸되는 생명주기 (life cycle) 을 갖는다 . 만약 생명주기가 없다면 프로그램이 종료되지 않는 한 영원히 메모리 공간을 점유하게 된다

> 전역변수 선언문은 어디에 있든 상관없이 가장 먼저 실행된다.
>
> 반면, 지역변수(함수 내부의 변수)는 함수가 호출되면 다른 문들이 실행되기 이전에 선언문들이 실행되면서 변수가 생된다. 그리고 함수가 종료되면 변수도 소멸되어 생명주기가 종료된다.

즉, 지역 변수의 생명주기는 함수의 생명주기와 일치한다 

보통 지역 변수의 생명주기와 함수의 생명주기는 일치하지만, **지역변수가 함수보다 오래 생존하는 경우도 있다** (아래와 같은 경우- 클로저)

```js
function foo() {
  var x = 10;
  function bar() {
    consol.log(x);
  }
  return bar;
}
var bar = foo(); // 이미 foo함수는 호출되어 bar함수를 리턴하고 끝났지만 아래에서 bar함수를 리턴받아 호출해도 x가 잘 참조되어 10이 출력된다 
bar(); //10
```



### 1.2 전역 변수의 생명 주기

> 함수와 달리 전역 코드는 명시적인 호출없이 실행된다. 즉 함수 호출과 같이 전역코드를 실행하는 진입점이 없고 코드가 로드되자마자 곧바로 해석되고 실행된다 따라서 전역 변수의 생명주기는 전역 코드의 마지막 문이 실행되어 더이상 실행할 문이 없을 때 종료되고 전역변수도 소멸된다 

var 키워드로 선언한 전역변수는 전역 객체의 프로퍼티가 된다 이는 전역변수의 생명주기는 전역객체의 생명주기와 일치한다는 것이다.

> 브라우저 환경에서는 window가 전역 객체이므로 var 키워드로 선언한 전역 변수는 window 객체의 프로퍼티이다. 
>
> 브라우저 환경에서는 var 키워드로 선언한 전역변수는 웹페이지를 닫을 때까지 유효하다 



## 2. 전역 변수의 문제점

**암묵적 결합**

> 전역 코드는 코드 어디서든 참조하고 할당할 수 있는 변수이기 때문에 모든 코드가 참조하고 변경할 수 있는 암묵적 결합(implicit coupling)을 허용하는 것이다. 변수의 유효 범위가 클수록 코드의 가독성이 나빠지고 의도치 않게 상태가 변경될 수 있는 위험성도 높아진다.

**긴 생명주기**

> 전역변수는 생명주기가 길다. 따라서 메모리 리소스도 오랫동안 점유한다
>
> 또한 var 키워드는 변수의 중복 선언을 허용하기 때문에 변수를 중복으로 선언할 수 있고, 이로 인해 의도치 않은 재할당이 이뤄질 수 있다 

```js
var x = 1;

// ...
// 변수의 중복 선언, 기존 변수에 값을 재할당한다
var x = 100;
console.log(x); //100
```



**스코프 체인 상에서 종점에 존재**

> 전역 변수는 스코프 체인 상에서 가장 끝점에 위치하기 때문에, 변수를 검색할때 가장 마지막에 검색된다. 즉, **변수 검색 속도가 가장 느리다.** 검색 속도의 차이가 크진 않지만 차이는 분명히 있다

**네임스페이스 오염**

> 자바스크립트의 가장 큰 문제점 중 하나는 **파일이 분리되어 있어도 하나의 전역 스코프를 공유한다**는 것이다.
> 다른 파일 내에서 동일한 이름으로 전역 변수나 전역 함수가 존재하는 경우 예상치 못한 결과를 가져올 수 있다.
> 이는 뒤에 나오는 모듈 패턴이나 ES6 모듈을 활용하여 해결할 수 있다.



## 3. 전역 변수의 사용을 억제하는 방법

전역 변수의 무분별한 사용은 많은 위험을 초래할 수 있다.
전역 변수의 사용을 억제하는 방법을 알아보자.



### 3.1 즉시 실행 함수

**모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.**

```js
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
  // ...
}());

console.log(foo); // ReferenceError: foo is not defined
// foo는 지역 변수이기 때문에 바깥에서 참조할 수 없음
```



### 3.2 네임스페이스 객체

전역 네임스페이스 객체를 만들어 프로퍼티로 관리하는 방법이다. 어차피 전역 객체이기 때문에 그닥 좋은 방법은 아니다.

```js
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.name = 'Lee';

console.log(MYAPP.name); // Lee
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log(MYAPP.person.name); // Lee
```



### 3.3 모듈 패턴

클로저를 활용해 클래스를 모방해서 만든 패턴이다.
전역 변수의 억제와 캡슐화까지 구현할 수 있다.

```js
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    },
    getNum() {
      return num;
    }
  };
}());

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined
console.log(Counter.getNum());   // 0
console.log(Counter.increase()); // 1
console.log(Counter.getNum());   // 1
console.log(Counter.increase()); // 2
console.log(Counter.getNum());   // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```



### 3.4 ES6 모듈

ES6 모듈을 사용하면 전역 변수를 사용할 수 없다. **ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.**
모듈 내에서 `var`키워드로 선언하더라도 전역 변수가 아니며 `window`객체의 프로퍼티도 아니다.
`script`태그에 `type="module"`어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로 동작한다.

```js
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```







# 15강 . let, const의 블록레벨스코프 



## 1. Var 키워드로 선언한 변수의 문제점

ES5까지 변수를 선언할 수 있는 방법은 `var`키워드가 유일했다. `var`키워드는 3가지 특징이 있는데 이는 다른 언어와 구별되는 독특한 특징으로 사용시 유의해야 한다.

### 1.1 변수 중복 선언 허용

`var`키워드로 선언한 변수는 중복 선언이 가능하다.

```js
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

`var`키워드로 `x`와 `y`변수가 중복 선언되었다. 중복 선언하면 초기화문 유무에 따라 다르게 동작한다. 초기화 문이 있는 경우 `var`키워드가 없는것 처럼 동작하고, 초기화 문이 있는경우 변수 선언문은 무시된다.



### 1.2 함수 레벨 스코프

`var`키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
함수 외부에서 `var`키워드로 선언하면 코드 블록`{ }`내에서 선언해도 모두 전역 변수가 된다.

```js
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

`for`문의 변수 선언문에서 선언해도 전역 변수가 된다.

```js
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```



### 1.3 변수 호이스팅

`var`키워드로 변수를 선언하면 **변수 호이스팅**에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 그래서 변수 선언문 이전에 참조할 수 있다. 단, 선언문 이전에 참조하는 경우 `undefined`를 반환한다.

```js
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

위와 같은 예제는 프로그램의 흐름상 맞지도 않고 가독성도 떨어뜨리므로 좋지 않다.



## 2. let 키워드

앞서 살펴본 `var`키워드의 단점을 보완하기 위해 ES6에서는 새로운 변수 선언 키워드인 `let`과 `const`를 도입했다. `var`키워드와의 차이점을 중심으로 `let`키워드를 비교해 보자.



### 2.1 변수 중복 선언 금지

`let`키워드는 중복 선언을 할 수 없다. 중복 선언하면 문법에러`SyntaxError`가 발생한다.

```js
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456;


let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```



### 2.2 블록 레벨 스코프

`var`키워드는 함수 레벨 스코프를 따른다.
하지만 `let`키워드는 모든 코드 블록(함수,`if`문, `for`문, `while`문, `try/catch`문 등)을 지역 스코프로 인정하는 블록 레벨 스코프`block-level scope`를 따른다.

```js
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

전역에 선언된 `foo`변수와 코드 블록 내에서 선언된 `foo`변수는 다른 별개의 변수다.



### 2.3 변수 호이스팅

`var`와 달리 `let`키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
하지만 동일하게 변수 호이스팅이 발생한다.

```js
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

이처럼 참조에러`ReferenceError`가 발생한다.
`var`키워드로 선언한 변수는 암묵적으로 **선언 단계**와 **초기화 단계**가 한번에 진행된다.
반면, `let`키워드로 선언한 변수는 **선언 단계**와 **초기화 단계**가 분리되어 진행된다.



`var`키워드로 선언한 변수

```js
// var 키워드로 선언한 변수는 런타임 이전에 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```



`let`키워드로 선언한 변수

```js
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
// 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

`let`키워드로 선언한 변수는 일시적 사각지대(`TDZ Temporal Dead Zone`)가 발생한다.
변수가 호이스팅된 스코프의 시작점부터 `let`키워드로 선언된 선언문을 만나기 전 까지의 구간을 일시적 사각지대 (TDZ)라고 부른다.
이 지점에서 변수를 참조하려고 하면 참조에러`ReferenceError`가 발생한다.

`let`키워드로 선언한 변수가 호이스팅 된다는 증거는 아래 예제를 통해 살펴 보자.

```js
let foo = 1; // 전역 변수

{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

만약 변수 호이스팅이 되지 않는다면 위 예제에서 전역 변수 `foo`값인 `1`을 출력해야 한다. 하지만 `let`키워드로 선언한 변수도 호이스팅이 발생하기 때문에 참조에러`ReferenceError`가 발생한다.

자바스크립트는 ES6에서 도입된 `let`, `const`를 포함해서 모든 선언(`var`, `let`, `const`, `function`, `function*`, `class` 등)을 호이스팅한다.



### 2.4 전역 객체와 let

`var`키워드로 전역 변수와 전역 함수, 그리고 선언하지 안흔 변수(암묵적 전역변수)는 모두 `window`객체의 프로퍼티가 된다. (전역 객체의 프로퍼티를 참조할 때`window`를 생략할 수 있다.) 하지만 `let`키워드로 선언된 변수는 전역 객체의 프로퍼티가 아니다.



`var` 키워드

```js
// 이 예제는 브라우저 환경에서 실행해야 한다.

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // ƒ foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // ƒ foo() {}
```



`let`키워드

```js
// 이 예제는 브라우저 환경에서 실행해야 한다.
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x); // undefined
console.log(x); // 1
```





## 3. const 키워드

`const`키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.
`const`키워드의 특징은 `let`키워드와 대부분 동일하다.
`let`키워드와 다른 점을 중심으로 살펴 보자.3.1 선언과 초기화



### 3.1 선언과 초기화

`const`키워드로 선언한 변수는 **반드시 선언과 동시에 초기화 해야 한다.**
그렇지 않으면 문법에러`SyntaxError`가 발생한다.

```js
const foo = 1;
const foo; // SyntaxError: Missing initializer in const declaration
```

`const`키워드로 선언한 변수도 `let`과 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지만 발생하지 않는것 처럼 동작한다.

```js
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```



### 3.2 재할당 금지

`const`키워드로 선언한 변수는 재할당이 금지된다.

```js
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```



### 3.3 상수

`const`키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. 원시 값은 변경 불가능한`immutable value`이므로 재할당 없이 값을 변경할 수 있는 방법이 없기 때문이다. 이러한 특징을 이용해 `const`키워드를 상수를 표현하는데 사용하기도 한다.

일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다. 여러 단어로 이뤄진 경우 언더스코어`_`로 구분해 스네이크 케이스로 표현하는 것이 일반적이다.

```js
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```



### 3.4 const 키워드와 객체

`const`키워드로 선언된 변수에 원시 값을 할당하면 변경할 수 없다.
하지만 객체를 할당하는 경우 값을 변경할 수 있다. 객체는 변경 가능한 값`mutable value`이기 때문이다. 재할당 없이도 직접 변경이 가능하다.
즉, `const`키워드는 재할당을 금지할 뿐 **불변**을 의미하지는 않는다.

```js
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```



## 4. var vs let vs const 

변수 선언시 기본적으로 `const`를 사용한다.
재할당이 필요한 경우에 한정하여 `let`을 사용하는 것이 좋다.

`var`, `let`, `const`키워드 사용시 권장사항

- ES6를 사용한다면 `var`는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 `let`키워드를 사용하고, 스코프는 최대한 좁게 만든다.
- 변경이 없고 읽기 전용으로 사용하는 원시 값과 객체에는 `const`를 사용한다.(재할당이 안되므로 안전하다.)

※ 변수를 선언하는 시점에는 재할당이 필요할지 모르는 경우가 많다. 그래서 변수를 선언할 때 일다는 `const`키워드를 사용해 선언하자. 코드를 만들다가 재할당이 필요하면 그때 `let`키워드로 변경해도 결코 늦지 않다.