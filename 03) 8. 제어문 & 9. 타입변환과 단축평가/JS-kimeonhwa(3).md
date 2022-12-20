제어문 / 타입변환과 단축평가



## 8. 제어문

> 제어문(`control flow statement`)은 조건에 따라 코드 블록을 실행(조건문)하거나 반복실행(반복문)할 때 사용한다. 일반적인 코드는 위에서 아래로 순차적으로 실행하지만, 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.



### 1. 블록문

> 블록문(`block statement`)은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다. 
>
> 블록문은 하나의 실행 단위로 취급하며, 단독으로 사용하기도 하나 일반적으로 제어문이나 함수를 정의할 때 사용 한다.
>
> **문의 끝에는 세미콜론을 붙이는 것이 일반적이다.** 하지만 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 **블록문의 끝에는 세미콜론을 붙이지 않는다.**

```js
// 블록문
{
  var foo = 10;
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



### 2. 조건문

> 조건문(`conditioonal statement`)은 주어진 조건식(`condition expression`)의 평가결과에 따라 코드블록(블록문의 실행을 결정한다. 조건식은 불리언 값으로 평가될수 있는 표현식이다.

#### if~else 문

```js
var num = 2;
var test;

// if 문
if (num > 0) {
  test = '양수'; // 음수를 구별할 수 없다
}
console.log(test); // 양수

// if...else 문
if (num > 0) {
  test = '양수';
} else {
  test = '음수'; // 0은 음수가 아니다.
}
console.log(test); // 양수

// if...else if 문
if (num > 0) {
  test = '양수';
} else if (num < 0) {
  test = '음수';
} else {
  test = '영';
}
console.log(test); // 양수
```

=> if~else 문은 삼항 조건 연산자로 바꿔 쓸 수 있다



#### switch문

> `switch`문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 `case`문으로 실행 흐름을 옮긴다. `switch`문의 표현식과 일치하는 `case`문이 없다면 실행 순서는 `default`문으로 이동한다.

```js
// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'January';
  case 2: monthName = 'February';
  case 3: monthName = 'March';
  case 4: monthName = 'April';
  case 5: monthName = 'May';
  case 6: monthName = 'June';
  case 7: monthName = 'July';
  case 8: monthName = 'August';
  case 9: monthName = 'September';
  case 10: monthName = 'October';
  case 11: monthName = 'November';
  case 12: monthName = 'December';
  default: monthName = 'Invalid month';
}

console.log(monthName); // Invalid month
```

=> 위 예제는 `November`가 출력되지 않고 `Invalid month`가 출력된다 이는 평가 결과와 일치하는 `case`문으로 실행 흐름이 이동하여 문을 실행한 것은 맞지만 폴스루(`fall through`)가 발생되어 이후의 모든 `case`문을 실행하고 `default`문까지 실행한다. 그래서 마지막 `default`에서 `Invalid month`가 할당되어 출력된 것이다.
이러한 문제는 `break`문을 사용하면 해결된다.

```js
// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'January'; break;
  case 2: monthName = 'February'; break;
  case 3: monthName = 'March'; break;
  case 4: monthName = 'April'; break;
  case 5: monthName = 'May'; break;
  case 6: monthName = 'June'; break;
  case 7: monthName = 'July'; break;
  case 8: monthName = 'August'; break;
  case 9: monthName = 'September'; break;
  case 10: monthName = 'October'; break;
  case 11: monthName = 'November'; break;
  case 12: monthName = 'December'; break;
  default: monthName = 'Invalid month'; break;
}

console.log(monthName); // November

```

=> 일부러 폴스루(`fall through`)를 활용하는 경우도 있다.

```js
var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;

switch (month) {
  case 1: case 3: case 5: case 7: case 8: case 10: case 12:
    days = 31;
    break;
  case 4: case 6: case 9: case 11:
    days = 30;
    break;
  case 2:
    // 윤년 계산 알고리즘
    // 1. 연도가 4로 나누어떨어지는 해(2000, 2004, 2008, 2012, 2016, 2020...)는 윤년이다.
    // 2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해(2000, 2100, 2200...)는 평년이다.
    // 3. 연도가 400으로 나누어떨어지는 해(2000, 2400, 2800...)는 윤년이다.
    days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
    break;
  default:
    console.log('Invalid month');
}

console.log(days); // 29
```



### 3. 반복문

> 반복문(`loop statement`)은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다. 거짓일 때까지 반복한다.



#### for문

```js
for(변수 선언문 또는 할당문; 조건식; 증감식){
    조건식이 참인 경우 반복 실행될 문;
}

for (var i = 0; i < 2; i++) {
  console.log(i);
}
```



#### while문

> `while`문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속 해서 반복 실행한다. `for`문은 반복 횟수가 명확할 때 주로 사용하고 `while`문은 반복횟수가 불명확할 때 주로 사용한다.

```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}

//--------------------------
var count = 0;

// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
} // 0 1 2
```



#### do~while문

> `do...while`문은 코드 블록을 먼저 실행하고 조건식을 평가한다. 코드 블록은 무조건 한 번 이상 실행된다.

```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count);
  count++;
} while (count < 3); // 0 1 2
```



### break 문

> `switch`문과 `while`문에서 보았듯이 `break`문을 사용하면 코드 블록을 탈출 한다. 정확히 말하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문 또는 `switch`문의 코드 블록을 탈출한다.

```js
if (true) {
  break; // Uncaught SyntaxError: Illegal break statement
}
```

=> 레이블 문(`label statement`)이란 식별자가 붙은 문을 말한다.

```js
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log('foo');


// foo라는 식별자가 붙은 레이블 블록문
foo: {
  console.log(1);
  break foo; // foo 레이블 블록문을 탈출한다.
  console.log(2);
}

console.log('Done!');

```

=> 보통 이중 포문을 탈출하기 위해 종종 사용된다.

```js
// outer라는 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출한다.
    if (i + j === 3) break outer;
    console.log(`inner [${i}, ${j}]`);
  }
}

console.log('Done!');
```

=> 레이블 문의 사용은 `for`문을 탈출할때 많이 사용되지만 그 밖의 경우에는 일반적으로 권장하지 않는다. 레이블 문을 사용하면 프로그램의 흐름이 복잡해져 가독성이 나빠지고 오류를 발생시킬 가능성이 높아지기 때문이다.



### continue문

> `continue`문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. `break`문 처럼 반복문을 탈출하지는 않는다.

```js
var string = 'Hello World.';
var search = 'l';
var count = 0;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if (string[i] !== search) continue;
  count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}

console.log(count); // 3

// 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
const regexp = new RegExp(search, 'g');
console.log(string.match(regexp).length); // 3
```



```js
// continue 문을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
for (var i = 0; i < string.length; i++) {
  // 'l'이면 카운트를 증가시킨다.
  if (string[i] === search) {
    count++;
    // code
    // code
    // code
  }
}

// continue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 카운트를 증가시키지 않는다.
  if (string[i] !== search) continue;

  count++;
  // code
  // code
  // code
}
```





## 9. 타입변환과 단축평가



### 1. 타입변환

> 자바스크립트의 모든 값은 타입이 있다.
> 개발자가 의도적으로 값의 타입을 변환하는 `명시적 타입변환(explicit coercion)`과 의도와 상관없이 자바스크립트 엔진에 의해 변환되는 `암묵적 타입변환(implicit coercion)`이 있다.

- 명시적 타입변환(explicit coercion)은 `타입 캐스팅(type casting)`이라고도 한다.

```js
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```



- 암묵적 타입변환(implicit coercion)은 `타입 강제 변환(type coercion)`이라고도 한다.

```js
var x = 10;

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + '';
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

> 명시적 타입변환이나 암묵적 타입변환은 기존 원시 값을 직접 변경하는 것은 아니다.
원시 값은 변경 불가능한 값(`immutable value`)이므로 변경할 수 없다.
타입 변환이란 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.

- 위 예제에서 `x + ''` 표현식은 평가하기 위해 `x`변수의 숫자 값을 바탕으로 새로운 문자열 값 `'10'`을 생성하고 이것으로 표현식 `'10' + ''`를 평가한다.

- 명시적 타입변환은 타입을 변경하겠다는 개발자의 의지가 코드에 드러나지만,
  암묵적 타입 강제변환은 자바스크립트 엔진에 의해 암묵적으로 드러나지 않게 변환되기 때문에 코드에 명백히 나타나지 않는다.
  그렇기 때문에 주의해서 사용해야 한다.



### 2. 암묵적 타입 변환

> 자바스크립트 엔진은 표현식을 평가할 대 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있다.

```js
// 피연산자가 모두 문자열 타입이어야 하는 문맥
'10' + 2 // -> '102'

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * '10' // -> 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0 // -> true
if (1) { }
```



#### 문자열 타입으로 변환

> `+` 연산자를 사용하면 문자열 타입으로 변환된다. `+` 연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작한다.

```js
1 + '2' // -> "12"
```



- ES6에 도입된 템플릿 리터럴의 표현식 삽입도 평가 결과를 문자열 타입으로 암묵적 변환한다.

```js
`1 + 1 = ${1 + 1}` // -> "1 + 1 = 2"
```



- 문자열 타입으로 암묵적 타입변환 되는 예

```js
// 숫자 타입
0 + ''         // -> "0"
-0 + ''        // -> "0"
1 + ''         // -> "1"
-1 + ''        // -> "-1"
NaN + ''       // -> "NaN"
Infinity + ''  // -> "Infinity"
-Infinity + '' // -> "-Infinity"

// 불리언 타입
true + ''  // -> "true"
false + '' // -> "false"

// null 타입
null + '' // -> "null"

// undefined 타입
undefined + '' // -> "undefined"

// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // -> "[object Object]"
Math + ''           // -> "[object Math]"
[] + ''             // -> ""
[10, 20] + ''       // -> "10,20"
(function(){}) + '' // -> "function(){}"
Array + ''          // -> "function Array() { [native code] }"
```



#### 숫자 타입으로 변환

> 산술 연산자중 `+`제외한 연산자 `-`, `*`, `/`, `%` 를 사용하면 숫자 타입으로 변환된다.

```js
1 - '1'   // -> 0
1 * '10'  // -> 10
1 / 'one' // -> NaN
13 % '5' // -> 3
```

- 또한 비교 연산자도 숫자 타입으로 변환한다.

````js
'1' > 0  // -> true
````

- `+`단항 연산자를 문자열 앞에 사용하면 숫자 타입의 값으로 변환한다.

```js
// 문자열 타입
+''       // -> 0
+'0'      // -> 0
+'1'      // -> 1
+'string' // -> NaN

// 불리언 타입
+true     // -> 1
+false    // -> 0

// null 타입
+null     // -> 0

// undefined 타입
+undefined // -> NaN

// 심벌 타입
+Symbol() // -> ypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}             // -> NaN
+[]             // -> 0
+[10, 20]       // -> NaN
+(function(){}) // -> NaN
```



#### 불리언 타입으로 변환

> `if`문의 조건식은 불리언 타입으로 암묵적 타입 변환 한다.

```js
if ('')    console.log('1');
if (true)  console.log('2');
if (0)     console.log('3');
if ('str') console.log('4');
if (null)  console.log('5');

// 2 4
```

=> 자바스크립트 엔진은 불리언 타입이 아닌 값을 `Truthy`값(참으로 평가되는 값) 또는 `Falsy`값(거짓으로 평가되는 값)으로 구분한다.



False 값

- `false`
- `undefined`
- `null`
- `0`
- `-0`
- `NaN`
- `''`(빈 문자열)

```js
// 아래의 조건문은 모두 코드 블록을 실행한다.
if (!false)     console.log(false + ' is falsy value');
if (!undefined) console.log(undefined + ' is falsy value');
if (!null)      console.log(null + ' is falsy value');
if (!0)         console.log(0 + ' is falsy value');
if (!NaN)       console.log(NaN + ' is falsy value');
if (!'')        console.log('' + ' is falsy value');
```

- `Falsy`값 이외의 모든 값은 `true`로 평가되는 `Truthy`값이다.

```js
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

// 모두 true를 반환한다.
isFalsy(false);
isFalsy(undefined);
isFalsy(null);
isFalsy(0);
isFalsy(NaN);
isFalsy('');

// 모두 true를 반환한다.
isTruthy(true);
isTruthy('0'); // 빈 문자열이 아닌 문자열은 Truthy 값이다.
isTruthy({});
isTruthy([]);
```



### 3. 명시적 타입 변환

> 개발자의 의도에 따라 명시적으로 타입을 변경하는 방법이다.
> 표준 빌트인 생성자 함수(`String`, `Number`, `Boolean`)를 `new`연산자 없이 호출하는 방법 이 있고,
> 빌트인 메서드를 사용하는 방법이 있다.



#### 문자열 타입으로 변환

> 문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법
>
> - `String` 생성자 함수를 `new` 연산자 없이 호출하는 방법
> - `Object.prototype.toString` 메서드를 사용하는 방법
> - 문자열 연결 연산자를 이용하는 방법

```js
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1);        // -> "1"
String(NaN);      // -> "NaN"
String(Infinity); // -> "Infinity"
// 불리언 타입 => 문자열 타입
String(true);     // -> "true"
String(false);    // -> "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString();        // -> "1"
(NaN).toString();      // -> "NaN"
(Infinity).toString(); // -> "Infinity"
// 불리언 타입 => 문자열 타입
(true).toString();     // -> "true"
(false).toString();    // -> "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
1 + '';        // -> "1"
NaN + '';      // -> "NaN"
Infinity + ''; // -> "Infinity"
// 불리언 타입 => 문자열 타입
true + '';     // -> "true"
false + '';    // -> "false"
```

- ※ 참고(new 연산자 사용 여부에 따라 생성 타입이 달라진다.)

```js
// new 연산자를 사용하지 않으면 원시값이 생성된다.
var primitive = String('a');
typeof primitive // 'string'

// new 연산자와 함께 사용하면 object 타입이 생성된다.
var obj = new String('a');
typeof obj // 'object'

```



#### 숫자 타입으로 변환

> 숫자 타입이 아닌 값을 숫자 타입으로 변환 하는 방법
>
> - `Number` 생성자 함수를 `new`연산자 없이 호출하는 방법
> - `parseInt, parseFloat` 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
> - `+` 단항 산술 연산자를 이용하는 방법
> - `*` 산술 연산자를 이용하는 방법

```js
 // 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
Number('0');     // -> 0
Number('-1');    // -> -1
Number('10.53'); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true);    // -> 1
Number(false);   // -> 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
parseInt('0');       // -> 0
parseInt('-1');      // -> -1
parseFloat('10.53'); // -> 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+'0';     // -> 0
+'-1';    // -> -1
+'10.53'; // -> 10.53
// 불리언 타입 => 숫자 타입
+true;    // -> 1
+false;   // -> 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
'0' * 1;     // -> 0
'-1' * 1;    // -> -1
'10.53' * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1;    // -> 1
false * 1;   // -> 0
```



#### 불리언 타입으로 변환

> 불리언 타입이 아닌 값을 불리언 타입으로 변환
>
> - `Boolean`생성자 함수를 `new`연산자 없이 호출하는 방법
> - `!`부정 논리 연산자를 두 번 사용하는 방법

```js
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
Boolean('x');       // -> true
Boolean('');        // -> false
Boolean('false');   // -> true
// 숫자 타입 => 불리언 타입
Boolean(0);         // -> false
Boolean(1);         // -> true
Boolean(NaN);       // -> false
Boolean(Infinity);  // -> true
// null 타입 => 불리언 타입
Boolean(null);      // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({});        // -> true
Boolean([]);        // -> true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
!!'x';       // -> true
!!'';        // -> false
!!'false';   // -> true
// 숫자 타입 => 불리언 타입
!!0;         // -> false
!!1;         // -> true
!!NaN;       // -> false
!!Infinity;  // -> true
// null 타입 => 불리언 타입
!!null;      // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{};        // -> true
!![];        // -> true
```



### 4. 단축 평가

#### 논리 연산자를 사용한 단축 평가

> 논리합`||` 또는 논리곱`&&`연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다.

```js
1===1 && 'a'==='a' // true

'Cat' && 'Dog' // -> "Dog"
```

- 논리곱`&&`연산자는 두 개의 피연산자가 모두 `true`로 평가될 때 `true`를 반환한다.
  좌항에서 우항으로 평가가 진행된다.
  피 연산자가 불리언 타입이 아닐경우 값이 평가된다.

- 위 예제에서 첫 번재 피연산자 `'Cat'`은 `Truthy`값이므로 `true`로 평가된다. 하지만 이 시점까지는 위 표현식을 평가할 수 없다. 두 번째 피연산자 까지 평가해야 표현식을 평가할 수 있다.
  따라서 두 번째 피연산자, 즉 `'Dog'`까지 평가하고 평가 결과인 `'Dog'`문자열을 그대로 반환한다.

- 논리합`||`연산자는 두 개의 피연산자 중 하나만 `true`로 평가되어도 `true`를 반환한다.
  좌항에서 우항으로 평가를 진행한다.

```js
// 논리합(||) 연산자
'Cat' || 'Dog'  // -> "Cat"
false || 'Dog'  // -> "Dog"
'Cat' || false  // -> "Cat"

// 논리곱(&&) 연산자
'Cat' && 'Dog'  // -> "Dog"
false && 'Dog'  // -> false
'Cat' && false  // -> false
```



- 어떤 조건이 `Truthy`값인 경우 논리곱`&&`연산자 표현식으로 `if`문을 대체할 수 있다.

```js
var done = true;
var message = '';

// 주어진 조건이 true일 때
if (done) message = '완료';

// if 문은 단축 평가로 대체 가능하다.
// done이 true라면 message에 '완료'를 할당
message = done && '완료';
console.log(message); // 완료
```

- 조건이 `Falsy`값 인경우 논리합`||`연산자 표현식으로 `if`문을 대체할 수 있다.

```js
var done = false;
var message = '';

// 주어진 조건이 false일 때
if (!done) message = '미완료';

// if 문은 단축 평가로 대체 가능하다.
// done이 false라면 message에 '미완료'를 할당
message = done || '미완료';
console.log(message); // 미완료

```

- 삼항 조건 연산자는 `if...else`문을 대체 할 수 있다.

```js
var done = true;
var message = '';

// if...else 문
if (done) message = '완료';
else      message = '미완료';
console.log(message); // 완료

// if...else 문은 삼항 조건 연산자로 대체 가능하다.
message = done ? '완료' : '미완료';
console.log(message); // 완료

```

- 객체를 가리키기를 기대하는 변수가 `null`또는 `undefined`가 아닌지 확인하고 프로퍼티를 참조할 때
  단축 평가를 사용하면 에러없이 사용할 수 있다.

```js
// 그냥 참조
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null


// 단축 평가 사용
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // -> null
```

- 함수 매개변수에 기본값을 설정할 때
  함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 `undefined`가 할당되는데, 단축 평가를 사용하면 이를 방지할 수 있다.

```js
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || '';  // 단축 평가
  return str.length;
}

getStringLength();     // -> 0
getStringLength('hi'); // -> 2


// ES6의 매개변수의 기본값 설정
function getStringLength(str = '') {
  return str.length;
}

getStringLength();     // -> 0
getStringLength('hi'); // -> 2
```





#### 옵셔널 체이닝 연산자

> ES11(ECMAScript2020)에 도입된 `옵셔널 체이닝(optional chaining)` 연산자 `?.`는 좌항의 피연산자가 `null`또는 `undefined`인 경우 `undefined`를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```js
var elem = null;

// elem이 null 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
var value = elem?.value;
console.log(value); // undefined
```



- 옵셔널 체이닝 연산자가 도입되기 전에는 논리곱`&&`을 사용했었다.
  하지만 논리곱`&&`연산자는 옵셔널 체이닝 연산자와는 다르게 `Falsy`(`false`, `undefined`, `0`, `NaN`, `' '`)값이면 좌항의 피연산자를 반환 해버리기 때문에 조금 문제가 있다.

```js
var elem = null;

// elem이 Falsy 값이면 elem으로 평가되고 elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value;
console.log(value); // null



var str = '';

// 문자열의 길이(length)를 참조한다.
var length = str && str.length;

// 문자열의 길이(length)를 참조하지 못한다.
console.log(length); // ''
```

- 옵셔널 체이닝 연산자`?.`는 좌항의 피연산자가 `false`로 평가되는 `Falsy`(`false`, `undefined`, `0`, `NaN`, `' '`)값이라도 `null` 또는 `undefined`가 아니면 우항의 프로퍼티 참조를 이어간다.

```js
var str = '';

// 문자열의 길이(length)를 참조한다. 이때 좌항 피연산자가 false로 평가되는 Falsy 값이라도
// null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.
var length = str?.length;
console.log(length); // 0
```



#### null 병합 연산자

> ES11(ECMAScript2020)에 도입된 `null 병합(nullish coalescing)`연산자 `??`는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

```js
// 좌항의 피연산자가 null 또는 undefined이면 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
var foo = null ?? 'default string';
console.log(foo); // "default string"
```



- null 병합 연산자도 도입되기 이전에는 논리합`||`연산자를 사용했는데 논리곱`&&`연산자와 동일하게 모든 좌항의 피연산자가 `Falsy`(`false`, `undefined`, `0`, `NaN`, `' '`)값인 경우 우항의 피연산자를 반환하여 예기치 않은 동작이 발생할 수 있다.

```js
// Falsy 값인 0이나 ''도 기본값으로서 유효하다면 예기치 않은 동작이 발생할 수 있다.
var foo = '' || 'default string';
console.log(foo); // "default string"


// 좌항의 피연산자가 Falsy 값이라도 null 또는 undefined이 아니면 좌항의 피연산자를 반환한다.
var foo = '' ?? 'default string';
console.log(foo); // ""
```



