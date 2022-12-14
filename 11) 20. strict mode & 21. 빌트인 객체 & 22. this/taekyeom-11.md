# 20장 strice mode

## 20.1 strict mode란

```js
function foo() {
  x = 10;
}
foo();

console.log(x); // ?
```

전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용할 수 있다. 이러한 현상을 **암묵적 전역**이라 한다.

개발자의 의도와는 상관 없이 발생한 암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 크다. 따라서 반드시 var, let, const 키워드를 사용하여 변수를 선언한 다음 사용해야 한다.

ES5부터 strict mode(엄격 모드)가 추가되었다.

strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

## 20.2 strict mode의 적용

strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 `'use strict';`를 추가한다.

- 전역 선두에 추가하면 스크립트 전체에 strict mode가 적용된다.

- 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.

## 20.3 전역에 strict mode를 적용하는 것은 피하자

스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.

하지만 strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.

이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 발생할 수 있다.

따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전연

선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

### 20.5.2 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.

### 20.5.3 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.

### 20.5.4 with 문의 사용

with 문을 사용하면 SyntaxError가 발생한다. with 문은 전달된 객체를 스코프 체인에 추가한다.

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다. 이때 에러는 발생하지 않는다.

### 20.6.2 arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

# 21장 빌트인 객체

## 21.1 자바스크립트 객체의 분류

- 표준 빌트인 객체
- 호스트 객체
- 사용자 정의 객체

## 21.2 표준 빌트인 객체

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 잇는 생성자 함수 객체다.

생성자 함수 객체인 표준 빌트인 객체는 프로토타입 매서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

## 21.3 원시값과 래퍼 객체

문자열, 숫자, 불리언 값에 대한 객체 처럼 접근하면 생성되는 임시 객체를 래퍼 객체라 한다.

## 21.4 전역 객체

### 21.4.1 빌트인 전역 프로퍼티

Infinity 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 갖는다.

NaN 프로퍼티는 숫자가 아님을 나타내는 숫자값 NaN을 갖는다.

undefined 프로퍼티는 원시 타입 undefined를 값으로 갖는다.

### 21.4.2 빌트인 전역 함수

eval 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 받는다. 전달받은 문자열 코드가 표현식이라면 eval 함수는 문자열 코드를 런타임에 평가하여 값을 생성하고, 전달받은 인수가 표현식이 아닌 문이라면 eval 함수는 문자열 코드를 런타임에 실행한다.

isFinite 전달받은 인수가 정상적인 유한수인지 검사하여 유한수이면 true를 반환하고, 문한수이면 false를 반환한다.

isNaN 전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환한다. 전달 받은 인수의 타입이 숫자가 아닌 경우 숫자로 타입 변환한 후 검사를 수행한다.

parseFloat 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.

parseInt 전달받은 문자열 인수를 정수로 해석하여 반환한다.

encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.

decodeURI 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

encodeURIComponent 함수는 URI 구성 요소를 인수로 전달받아 인코딩한다.

decodeURIComponent 함수는 매개변수로 전달된 URI 구성 요소를 디코딩한다.

### 21.4.3 암묵적 전역

# 22장 this

## 22.1 this 키워드

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**

## 22.2 함수 호출 방식과 this 바인딩

this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

### 22.2.1 일반 함수 호출

일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.

### 22.2.2 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.

### 22.2.3 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩 된다.

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드다. 즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다. apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

Function.prototype.bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않는다. 다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.
