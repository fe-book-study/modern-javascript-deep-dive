# 8. 제어문 & 9. 타입변환과 단축평가

## 8장 제어문

제어문은 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용한다.

일반 적으로 코드는 위에서 아래 방향으로 순차적으로 실행된다. 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.

### 8.1 블록문

블록문은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다.

자바스크립트는 블록문을 하나의 실행 단위로 취급한다.

블록문은 단독으로 사용할 수도 있으니 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.

```js
// 블록문
{
  var for = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}


// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

### 8.2 조건문

조건문은 주어진 조건식의 평가 결과에 따라 코드 블록(블록문)의 실행을 결정한다.

조건식은 불리언 값으로 평가될 수 있는 표현식이다.

자바스크립트는 `if ... else` 문과 `switch` 문으로 두 가지 조건문을 제공한다.

#### 8.2.1 if ... else 문

```js
if (조건식) {
  // 조건식이 참이면 이 코드 블록이 실행된다.
} else {
  // 조건식이 거짓이면 이 코드 블록이 실행된다.
}

if (조건식1) {
  // 조건식1이 참이면 이 코드 블록이 실행된다.
} else if (조건식2) {
  // 조건식2가 참이면 이 코드 블록이 실행된다.
} else {
  // 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
}

// 대부분의 if ... else 문은 삼항 조건 연산자로 바꿔 쓸 수 있다.
var x = 2;
var result;

if (x % 2) {
  result = "홀수";
} else {
  reesult = "짝수";
}

var x = 2;

var result = x % 2 ? "홀수" : "짝수";
```

#### 8.2.2 switch 문

`switch` 문의 표현식과 일치하는 `case` 문이 없다면 실행 순서는 `default` 문으로 이동한다.

`default` 문은 선택사항으로, 사용할 수도 있고 사용하지 않을 수도 있다.

```js
switch (표현식) {
  case 표현식1:
    // switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    // switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  default:
  // switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;
}
```

`break` 키워드로 구성된 `break` 문은 코드 블록에서 탈출하는 역할을 한다.

`break` 문이 없다면 `case` 문의 표현식과 일치하지 않더라도 실행 흐름이 다음 `case` 문으로 연이어 이동한다.

### 8.3 반복문

반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다.

그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다.

이 조건식이 거짓일 때까지 반복된다.

자바스크립트는 세 가지 반복문인 `for` 문 `while` 문, `do ... while` 문을 제공한다.

#### 8.3.1 for 문

```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
  조건식이 참인 경우 반복 실행된 문
}
```

#### 8.3.2 while 문

```js
var count = 0;

while (count < 3) {
  console.log(count);
  count++;
}

// 무한루프
while (true) { ... }
```

#### 8.3.3 do ... while 문

```js
var count = 0;

do {
  console.log(count);
  count++;
} while (count < 3);
```

### 8.4 break 문

`switch` 문과 `while` 문에서 살펴보았듯이 `break` 문은 코드 블록을 탈출한다.

좀 더 정확히 표현하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문(`for`, `for ... in`, `for ... of`, `while`, `do ... while`) 또는 `switch` 문의 코드 블록을 탈출한다.

레이불 문 반복문, `switch` 문의 코드 블록 외에 `break` 문을 사용하면 `SyntaxError`(문법 에러)가 발생한다.

### 8.5 continue 문

`continue` 문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.

`break` 문처럼 반복문을 탈출하지는 않는다.

## 9장 타입 변환과 단축 평가

### 9.1 타입 변환이란?

자바스크립트의 모든 값은 타입이 있다.

값의 타입은 개발자의 의도에 따라 다른 타입으로 변환할 수 있다.

개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅**이라 한다.

개발자가 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다.

이를 **암묵적 타입 변환** 또는 **타입 강제 변환**이라 한다.

### 9.2 암묵적 타입 변환

#### 9.2.1 문자열 타입으로 변환

`+` 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.

문자열 연결 연산자의 역할은 문자열을 만드는 것이다.

따라서 문자열 연결 연산자의 모든 피연산자는 코드의 문맥상 모두 문자열 타입이어야 한다.

<br>

자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에 문자열 타입이 아닌 피연산자를 문자열 타입으로 압묵적 타입 변환한다.

```js
// 숫자 타입
0 + ''              // "0"
-0 + ''             // "0"
1 + ''              // "1"
-1 + ''             // "-1"
NaN + ''            // "NaN"
Infinity + ''       // "Infinity"
-Infinity + ''      // "-Infinity"

// 불리언 타입
true + ''           // "true"
false + ''          // "false"

// null 타입
null + ''           // "null"

// undefined 타입
undefined + ''      // "undefined"

// Symbol 타입
(Symbol()) + ''     // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[10, 20] + ''       // "10, 20"
(function(){}) + '' // "function(){}"
Array + ''          // "function Array() { [native code] }"
```

#### 9.2.2 숫자 타입으로 변환

산술 연산자의 역할은 숫자 값을 만드는 것이다.

따라서 산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.

자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연사자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.

이때 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가 결과는 NaN이 된다.

```
// 문자열 타입
+''               // 0
+'0'              // 0
+'1'              // 1
+'string'         // NaN

// 불리언 타입
+true             // 1
+false            // 0

// null 타입
+null             // 0

// undefined 타입
+undefined        // NaN

+Symbol()         // TypeError: Cannot convert a Symbol value to a number

// referenc
+{}               // NaN
+[]               // 0
+[10, 20]         // NaN
+(function(){})   // NaN
```

#### 9.2.3 불리언 타입으로 변환

자바스크립트 엔진은 불리언 타입이 아닌 값을 `Truthy` 값(참으로 평가되는 값) 또는 `Falsy` 값(거짓으로 평가되는 값)으로 구분한다.

즉, 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 `Truthy`값은 `true`로, `Falsy` 값은 `false`로 암묵적 타입 변환된다.

`false`로 평가되는 `Falsy` 값

- false
- undefined
- null
- 0, -0
- NaN
- ''(빈 문자열)

`Falsy` 값 외의 모든 값은 모두 `true`로 평가되는 `Truthy` 값

### 9.3 명시적 타입 변환

#### 9.3.1 문자열 타입 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 다음과 같다.

1. `String` 생성자 함수를 `new` 연산자 없이 호출하는 방법
2. `Object.prototype.toString` 메서드를 사용하는 방법
3. 문자열 연결 연산자 이용하는 방법

```js
// 숫자 타입 => 문자열 타입
String(1); // "1"
String(NaN); // "NaN"
string(Infinity); // "Infinity"
// 불리언 타입 => 문자열 타입
String(true); // "true"
String(false); // "false"

// 숫자 타입 => 문자열 타입
(1).toString(); // "1"
NaN.toString(); // "NaN"
Infinity.toString(); // "Infinity"
// 불리언 타입 => 문자열 타입
true.toString(); // "true"
false.toString(); // "false"

// 숫자 타입 => 문자열 타입
1 + ""; // "1"
NaN + ""; // "NaN"
Infinity + ""; // "Infinity"
// 불리언 타입 => 문자열 타입
true + ""; // true
false + ""; // false
```

#### 9.3.2 숫자 타입으로 변환

1. `Number` 생성자 함수를 `new` 연산자 없이 호출하는 방법
2. `parseInt` / `parseFloat` 함수를 사용하는 방법
3. `+` 단항 산술 연산자를 이용하는 방법
4. `*` 산술 연산를 이용하는 방법

```js
// 문자열 타입 => 숫자 타입
Number("0"); // 0
Number("-1"); // -1
Number("10.53"); // 10.53
// 불리언 타입 => 숫자 타입
Number(true); // 1
Number(false); // 0

// 문자열 타입 => 숫자 타입
parseInt("0"); // 0
parseInt("-1"); // -1
parseFloat("10.53"); // 10.53

// 문자열 타입 => 숫자 타입
+"0"; // 0
+"-1"; // -1
+"10.53"; // 10.53
//불리언 타입 => 숫자 타입
+true; // 1
+false; // 0

// 문자열 타입 => 숫자 타입
"0" * 1; // 0
"-1" * 1; // -1
"10.53" * 1; // 10.53
// 불리언 타입 => 숫자 타입
true * 1; // 1
false * 1; // 0
```

#### 9.3.3 불리언 타입으로 변환

1. `Boolean` 생성자 함수를 `new` 연산자 없이 호출하는 방법
2. `!` 부정 논리 연산자를 두 번 사용하는 방법

```js
// 문자열 타입 => 불리언 타입
Boolean("x"); // true
Boolean(""); // false
Boolean("false"); // true
// 숫자 타입 => 불리언 타입
Boolean(0); // false
Boolean(1); // true
Boolean(NaN); // false
Boolean(Infinity); // true
// null 타입 => 불리언 타입
Boolean(null); // false
// undefined 타입 => 불리언 타입
Boolean(undefined); // false
// 객체 타입 => 불리언 타입
Boolean({}); // true
Boolean([]); // true

// 문자열 타입 => 불리언 타입
!!"x"; // true
!!""; // false
!!"false"; // true
// 숫자 타입 => 불리언 타입
!!0; // false
!!1; // true
!!NaN; // false
!!Infinity; // true
// null 타입 => 불리언 타입
!!undefined; // false
// 객체 타입 => 불리언 타입
!!{}; // true
!![]; // true
```

### 9.4 단축 평가

#### 9.4.1 논리 연산자를 사용한 단축 평가

논리곱(`&&`) 연산자는 두 개의 피연산자가 모두 `true`로 평가될 때 `true`를 반환한다.

논리곱 연산자는 좌항에서 우항으로 평가가 진행된다.

논리합(`||`) 연산자는 두개의 피연산자 중 하나만 `true`로 평가되어도 `true`를 반환한다.

논립합 연산자도 좌항에서 우항으로 평가가 진행된다.

```js
true || anything; // true
false || anything; // anything
true && anything; // anything
false && anything; // false
```

#### 객체를 가리키기를 기대하는 변수의 값이 null 또는 undefined 인지 확인하고 프로퍼티를 참조할 때

```js
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null

var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // null
```

#### 함수 매개변수에 기본값을 설정할 때

```js
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || "";
  return str.length;
}

getStringLength(); // 0
getStringLength("hi"); // 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = "") {
  return str.length;
}

getStringLength(); // 0
getStrringLength("hi"); // 2
```

#### 9.4.2 옵셔널 체이닝 연산자

`ES11(ECMAScript2020)`에서 도입된 옵셔널 체이닝 연산자 `?.`는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined`를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```js
var elem = null;

// elem이 null또는 undefined이면 undefined 반환, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
var value = elem?.value;
console.log(value); // undefined
```

#### 9.4.3 null 병합 연산자

`ES11(ECMAScript2020)`에서 도입된 null 병합 연산자 `??`는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

```js
// 좌항의 연산자가 null 또는 undefined이면 우항의 피연산자를 반환하고,
// 그렇지 않으면 좌항의 피연산자를 반환한다.
var foo = null ?? "default string";
console.log(foo); // "default string"
```

```js
// 좌항의 연산자가 false로 평가되는 Falsy 값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환
var foo = "" ?? "default string";
console.log(foo); // ""
```
