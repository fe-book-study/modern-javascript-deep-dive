# 8. 제어문 & 9. 타입변환과 단축평가

## 8. 제어문

제어문(control flow statement)은 조건에 따라 코드 블록을 실행(조건문)하거나 반복실행(반복문)할 때 사용한다. 
일반적인 코드는 위에서 아래로 순차적으로 실행하지만, 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.

### 8.1 블록문

블록문(block statement)은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다. 
블록문은 하나의 실행 단위로 취급하며, 단독으로 사용하기도 하나 일반적으로 제어문이나 함수를 정의할 때 사용 한다.

문의 끝에는 세미콜론을 붙이는 것이 일반적이다. 
하지만 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않는다.

```javascript
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

### 8.2 조건문

조건문(conditioonal statement)은 주어진 조건식(condition expression)의 평가결과에 따라 코드블록(블록문의 실행을 결정한다. 
조건식은 불리언 값으로 평가될수 있는 표현식이다.
 
📍 if…else문

```javascript
var num = 2;
var kind;

// if 문
if (num > 0) {
  kind = '양수'; // 음수를 구별할 수 없다
}
console.log(kind); // 양수

// if...else 문
if (num > 0) {
  kind = '양수';
} else {
  kind = '음수'; // 0은 음수가 아니다.
}
console.log(kind); // 양수

// if...else if 문
if (num > 0) {
  kind = '양수';
} else if (num < 0) {
  kind = '음수';
} else {
  kind = '영';
}
console.log(kind); // 양수
```
if...else문은 7장 삼항 조건 연산자에서 본 삼항 조건 연산자로 바꿔 쓸수 있다.

📍 switch문

switch문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다. 
switch문의 표현식과 일치하는 case문이 없다면 실행 순서는 default문으로 이동한다.

```javascript
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
위 예제는 November가 출력되지 않고 Invalid month가 출력된다 이는 평가 결과와 일치하는 case문으로 실행 흐름이 이동하여 
문을 실행한 것은 맞지만 폴스루(fall through)가 발생되어 이후의 모든 case문을 실행하고 default문까지 실행한다. 
그래서 마지막 default에서 Invalid month가 할당되어 출력된 것이다.
이러한 문제는 break문을 사용하면 해결된다.

```javascript
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

일부러 폴스루(fall through)를 활용하는 경우도 있다.

```javascript
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

### 8.3 반복문

반복문(loop statement)은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 
그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다. 거짓일 때까지 반복한다.

1) for문

```javascript
for(변수 선언문 또는 할당문; 조건식; 증감식){
    조건식이 참인 경우 반복 실행될 문;
}

for (var i = 0; i < 2; i++) {
  console.log(i);
}
```

2) while문

while문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속 해서 반복 실행한다. 
for문은 반복 횟수가 명확할 때 주로 사용하고 while문은 반복횟수가 불명확할 때 주로 사용한다.

```javascript
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}
```

```javascript
var count = 0;

// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
} // 0 1 2
```

3) do…while문

do...while문은 코드 블록을 먼저 실행하고 조건식을 평가한다. 
코드 블록은 무조건 한 번 이상 실행된다.

```javascript
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count);
  count++;
} while (count < 3); // 0 1 2
```

### 8.4 break문

switch문과 while문에서 보았듯이 break문을 사용하면 코드 블록을 탈출 한다. 
정확히 말하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문 또는 switch문의 코드 블록을 탈출한다.

```javascript
if (true) {
  break; // Uncaught SyntaxError: Illegal break statement
}
```

레이블 문(label statement)이란 식별자가 붙은 문을 말한다.

```javascript
// foo라는 식별자가 붙은 레이블 블록문
foo: {
  console.log(1);
  break foo; // foo 레이블 블록문을 탈출한다.
  console.log(2);
}

console.log('Done!');
```
보통 이중 포문을 탈출하기 위해 종종 사용된다.

```javascript
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

레이블 문의 사용은 for문을 탈출할때 많이 사용되지만 그 밖의 경우에는 일반적으로 권장하지 않는다. 
레이블 문을 사용하면 프로그램의 흐름이 복잡해져 가독성이 나빠지고 오류를 발생시킬 가능성이 높아지기 때문이다.

### 8.5 continue문

continue문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. 
break문 처럼 반복문을 탈출하지는 않는다.

## 9. 타입 변환과 단축 평가

### 9.1 타입 변환이란?

자바스크립트의 모든 값은 타입이 있다.
개발자가 의도적으로 값의 타입을 변환하는 명시적 타입변환(explicit coercion)과 의도와 상관없이 
자바스크립트 엔진에 의해 변환되는 암묵적 타입변환(implicit coercion)이 있다.

명시적 타입변환(explicit coercion)은 타입 캐스팅(type casting)이라고도 한다.

```javascript
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

### 9.2 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 대 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있다.

1) 문자열 타입으로 변환

+ 연산자를 사용하면 문자열 타입으로 변환된다. 
+ + 연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작한다.

2) 숫자 타입으로 변환

산술 연산자중 +제외한 연산자 -, *, /, % 를 사용하면 숫자 타입으로 변환된다.


3) 불리언 타입으로 변환

if문의 조건식은 불리언 타입으로 암묵적 타입 변환 한다.

### 9.3 명시적 타입 변환

개발자의 의도에 따라 명시적으로 타입을 변경하는 방법이다.
표준 빌트인 생성자 함수(String, Number, Boolean)를 new연산자 없이 호출하는 방법 이 있고,
빌트인 메서드를 사용하는 방법이 있다.

1) 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

String 생성자 함수를 new 연산자 없이 호출하는 방법
Object.prototype.toString 메서드를 사용하는 방법
문자열 연결 연산자를 이용하는 방법

2) 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환 하는 방법

Number 생성자 함수를 new연산자 없이 호출하는 방법
parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
+ 단항 산술 연산자를 이용하는 방법
* 산술 연산자를 이용하는 방법

3) 불리언 타입으로 변환

불리언 타입이 아닌 값을 불리언 타입으로 변환

Boolean생성자 함수를 new연산자 없이 호출하는 방법
!부정 논리 연산자를 두 번 사용하는 방법

### 9.4 단축 평가

1) 논리 연산자를 사용한 단축 평가

논리합|| 또는 논리곱&&연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다.
논리곱&&연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다.
좌항에서 우항으로 평가가 진행된다.
피 연산자가 불리언 타입이 아닐경우 값이 평가된다.

2) 옵셔널 체이닝 연산자

ES11(ECMAScript2020)에 도입된 옵셔널 체이닝(optional chaining) 연산자 ?.는 
좌항의 피연산자가 null또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

3) null 병합 연산자Permalink

ES11(ECMAScript2020)에 도입된 null 병합(nullish coalescing)연산자 ??는 
좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
