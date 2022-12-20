# 20장 strict mode

### 20.1 strict mode란?

- 오타나 문법 지식의 미비로 인한 실수는 언제나 발생한다 -> 따라서, 오류를 줄여 안정적인 코드를 생산하기 위해서는 좀 더 `근본적인 접근`이 필요하다.
  - 다시 말해, 잠재적인 오류를 발생시키기 어려운 개발 환경을 만들고 그 환경에서 개발하는 것이 좀 더 근본적인 해결책이라고 할 수 있다.
  - 이를 지원하기 위해  ES5부터 `strict mode`가 추가된 것!!
- strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.
- ESLint 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다.

<br>

### 20.2 strict mode의 적용

---

- strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 `use strict`를 추가한다. 전역의 선두에 추가하면 스크립트 전체에 strict mode가 적용된다.

  ```javascript
  'use strict';
  
  function foo() {
      x = 10; //ReferenceError: x is not defined
  }
  foo();
  ```

<br>

### 20.3 전역에 strict mode를 적용하는 것은 피하자

---

- 전역에 적용한 strict mode는 스크립트 단위로 적용된다.
- strict mode 스크립트와 non-script mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다. 특히 외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않다.
  - 이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

<br>

### 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

---

- 함수 단위로도 strict mode를 적용할 수 있다.
- 하지만, 함수마다 strict mode를 적용하는 여부를 다르게 하는 것은 바람직하지 않으며 모든 함수에 일일이 적용하는 것은 번거로운 일이다.
- 따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

<br>

### 20.5 strict mode가 발생시키는 에러

#### 20.5.1 암묵적 전역

---

- 선언하지 않은 변수를 참조하면 `ReferenceError`가 발생한다.

  ```javascript
  (function () {
      'use strict';
      
      x = 1;
      console.log(x); // ReferenceError: x is not defined
  })
  ```

<br>

#### 20.5.2 변수, 함수, 매개변수의 삭제

---

- delete 연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError`가 발생한다.

  ```javascript
  (function () {
      'use strict';
      
      var x = 1;
      delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.
      
      function foo(a) {
          delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
      }
      delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
  }())
  ```



<br>

#### 20.5.3 매개변수 이름의 중복

---

- 중복된 매개변수 이름을 사용하면 `SyntaxError`가 발생한다.

  ```javascript
  (function () {
      'use strict';
      
      //SyntaxError: Duplicate parameter name not allowed in this context
      function foo(x, x) {
          return x + x;
      }
      console.log(foo(1, 2));
  }());
  ```

<br>

#### 20.5.4 with 문의 사용

---

- with 문을 사용하면 `SyntaxError`가 발생한다. with 문은 전달된 객체를 스코프 체인에 추가한다.
- with문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있다.

<br>

### 20.6 strict mode 적용에 의한 변화

#### 20.6.1 일반 함수의 this

---

- strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.

<br>

#### 20.6.2 arguments 객체

----

- strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.


# 21장. 빌트인 객체

### 21.1 자바스크립트 객체의 분류

---

- 표준 빌트인 객체

  - ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통기능을 제공한다.
  - 자바스크립트 실행 환경(브라우저 또는 Node.js 환경)과 관계없이 언제나 사용할 수 있다.
  - 표준 빌트인 객체는 전역 객체의 프로퍼티로서 제공된다. -> 따라서 별도의 선언 없이 전역 변수처럼 언제나 참조 가능

- 호스트 객체

  - ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경(브라우저 or Node .js)에서 추가로 제공하는 객체

  - 브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpRequest, fetch등과 같은 클라이언트 사이드 Web API를 호스트 객체로 제공한다.
  - Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공한다.

- 사용자 정의 객체

  - 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체



<br>

### 21.2 표준 빌트인 객체

---

- 자바스크립트는 Object, String, Number, Boolean, Symbol, Date등 40여 개의 표준 빌트인 객체를 제공한다.
- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체이다.
  - 생성자 함수 객체인 표준 빌트인 객체는 `프로토타입 메서드`와 `정적 메서드`를 제공
    - 해당 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 `prototype 프로퍼티`에 바인딩된 객체이다.
  - 생성자 함수 객체가 아닌 표준 빌트인 객체는 `정적 메서드`만 제공

    ```javascript
    // Number 생성자 함수에 의한 Number 객체 생성
    const numObj = new Number(1.5) // Number {1.5}
    
    // toFixed는 Number.prototype의 프로토타입 메서드다.
    // Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
    console.log(numObj.toFixed()); // 2
    
    // isInteger는 Number의 정적 메서드다.
    // Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
    console.log(Number.isInteger(0.5)); // false	
    ```

<br>

### 21.3 원시값과 래퍼 객체

---

**그렇다면, 표준 빌트인 생성자 함수가 존재하는 이유는 무엇일까?**

- 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 마치 객체처럼 동작한다.

  ```javascript
  const str = 'hello';
  
  // 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
  console.log(str.length); //5
  console.log(str.toUpperCase()); // HELLO
  ```

  - 이는, 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해주기 때문이다. -> 즉, 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

  - 이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 `래퍼 객체`라 한다.



- **ex) 문자열에 대해 마침표 표기법으로 접근하면 발생하는 상황**

  - 접근하는 순간 래퍼 객체인 String 생성자 함수의 `인스턴스`가 생성되고 문자열은 래퍼 객체의 `[[StringData]] 내부 슬롯`에 할당된다.

    ```javascript
    const str = 'hi';
    
    //원시 타입인 문자열이 래퍼객체인 String 인스턴스로 변환된다.
    console.log(str.length) // 2
    console.log(str.toUpperCase()); // HI
    
    // 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
    console.log(typeof str); // string
    ```

  - 이때 문자열 래퍼 객체인 String 생성자 함수의 `인스턴스`는 String.prototype의 메서드를 상속받아 사용할 수 있다.

  - 그 후 래퍼 객체의 처리가 종료되면 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

    ``` javascript
    // 1. 식별자 str은 문자열을 값으로 가지고 있다.
    const str = 'hello';
    
    // 2. 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
    // 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
    // 래퍼 객체에 name 프로퍼티가 동적 추가된다.
    str.name = 'song';
    
    // 3. 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
    // 이때 2번에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
    
    // 4. 식별자 str은 새롭게 암묵적으로 생성된(2번에서 생성된 래퍼 객체와는 다른) 래퍼 객체를 가리킨다.
    // 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
    console.log(str.name); // undefined
    
    // 5. 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
    // 이때, 4번에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
    console.log(typeof str, str) // string hello
    ```

- 문자열과 같이 숫자와 불리언도 동일하게 작동한다

<br>

### 21.4 전역 객체

---

- 전역 객체는 코드가 실행되기 `이전 단계`에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체다.

- 전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체, 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 `프로퍼티`로 갖는다.

  - 즉, 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체(표준 빌트인 객체와 호스트 객체)의 최상위 객체다.
  - **전역 객체가 최상위 객체라는 것은 프로토타입 상속 관계상에서 최상위 객체라는 의미가 아니다**
    - 전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다.

- 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.

- 전역 객체의 프로퍼티를 참조할 때 window(또는 global)를 생략할 수 있다.

- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 `암묵적 전역`, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.

  ```javascript
  // 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티다.
  bar = 2; // window.bar = 2
  console.log(window.bar) // 2
  ```

- let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)내에 존재하기 때문이다.

- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다. 여러 개의 script 태그를 통해 자바스크립트 코드를 분리해도 하나의 전역 객체 `window`를 공유하는 것은 변함이 없다.

<br>

#### 21.4.1 빌트인 전역 프로퍼티

---

- 빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다.

- 빌트인 전역 프로퍼티 종류
  - **Inifinity** 
    - 무한대를 나타내는 숫자값 Infinity를 갖는다.
  - **NaN**
    - 숫자가 아님을 나타내는 숫자값 NaN을 갖는다.
  - **undefined**
    - 원시 타입 undefined를 값으로 갖는다.

<br>

#### 21.4.2 빌트인 전역 함수

---

- 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드이다.

- 빌트인 전역 함수 종류

  - **eval**

    - 자바스크립트 코드를 나타내는 문자열을 인수로 전달받는다.
    - 전달받은 문자열 코드가 표현식이라면 eval 함수는 문자열 코드를 런타임에 평가하여 값을 생성한다.
    - 전달받은 인수가 표현식이 아닌 문이라면 eval 함수는 문자열 코드를 런타임에 실행한다.

    ```javascript
    // 표현식인 문
    eval('1 + 2;'); // 3
    
    // 표현식이 아닌 문
    eval('var x = 5;'); // undefined
    
    // eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언되었다.
    console.log(x); // 5
    
    // 객체 리터럴은 반드시 괄호로 둘러싼다.
    const o = oval('({ a: 1 })');
    console.log(o) // {a: 1}
    
    // 함수 리터럴은 반드시 괄호로 둘러싼다.
    const f = eval('(function() { return 1;})');
    console.log(f()); // 1
    
    ```

    - eval 함수는 자신이 호출된 위치에 해다하는 기존의 스코프를 런타임에 동적으로 수정한다.
      - 함수는 호출되면 런타임 이전에 먼저 함수 몸체 내부의 모든 선언문을 먼저 실행하고 그 결과를 스코프에 등록한다.
      - 하지만 해당 함수 내부에 eval 함수는 기존의 스코프를 런타임에 동적으로 수정한다.
    - **eval  함수의 사용을 금지해야하는 이유**
      - eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 매우 취약하기 때문에
      - eval 함수를 통해 실행되는 코드는 자바스크립트 엔진에 의해 최적화가 수행되지 않으므로 일반적인 코드 실행에 비해 처리 속도가 느리기 때문에

  - **isFinite**

    - 전달받은 인수가 정상적인 유한수인지 검사하여 유한수이면 true를 반환하고, 무한수이면 false를 반환하다.

  - **isNaN**

    - 전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환한다.

  - **parseFloat**

    - 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.

  - **parseInt**

    - 전달받은 문자열 인수를 정수로 해석하여 반환한다.

    - 두 번째 인수로 진법을 나타내는 기수를 전달할 수 있다. 기수를 지정하면 첫 번째 인수로 전달된 문자열을 해당 기수의 숫자로 해석하여 반환한다. 이때 반환값은 언제나 10진수다.

      ```javascript
      // '10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환한다.
      parseInt('10'); // 10
      // '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다.
      parseInt('10') // 2
      ```

  - **encodeURI**

    - 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.

      ```javascript
      // 완전한 URI
      const uri = 'http://naver.com?name=가나다&job=programmer&teacher';
      
      // encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
      const enc = encodeURI(uri);
      console.log(enc);
      // http://naver.com?name=%EG%23%D3%E3%FE%U5%O5%A8&job=programmer&teacher
      ```

  - **decodeURI**

    - 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩 한다.

      ```javascript
      // http://naver.com?name=%EG%23%D3%E3%FE%U5%O5%A8&job=programmer&teacher
      const enc = encodeURI(uri)
      
      const dec = decodeURI(enc);
      console.log(dec)
      // http://naver.com?name=가나다&job=programmer&teacher
      ```

  - **encodeURIComponent**

    - URI 구성 요소를 인수로 전달받아 인코딩한다.
    - 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다. 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다.

<br>

#### 21.4.3 암묵적 전역

---

```javascript
var x = 10; // 전역 변수
function foo() {
    // 선언하지 않은 식별자에 값을 할당
    y = 20; // window.y = 20;
}
foo();
// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

- 함수 foo 내부의 y처럼 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 된다.
- foo 함수가 호출되면 자바스크립트 엔진은 y 변수에 값을 할당하기 위해 먼저 `스코프 체인`을 통해 선언된 변수인지 확인한다. 이때 foo 함수의 스코프와 전역 스코프 어디에서도 y 변수의 선언을 찾을 수 없으므로 `참조 에러`가 발생한다. -> 하지만 자바스크립트 엔진은 y = 20을 `window.y = 20`으로 해석하여 전역 객체에 프로퍼티를 동적 생성한다. 이러한 현상을 **암묵적 전역**이라 한다.
  - y는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었을 뿐이다.

# 22장 this

## 22.1 this키워드

<br>

- 자바스크립트의 일반적인 함수들은 this라는 변수를 가지고 있다.
- this는 해당 함수를 메서드로 가지고 있는 객체 또는 해당 함수가 생성할 객체를 참조하는 변수이다.
- this는 함수 내에서 쓰일 때 의미가 있지만 전역에서도 참조할 수는 있다.
- this를 전역에서 참조할 경우 다음과 같이 동작한다.
  - 브라우저에서 동작할 경우 this는 전역 객체인 window를 참조한다.
  - node REPL에서 this는 전역 객체인 global을 참조한다.
  - commonJS의 모듈 시스템을 따르는 node 환경에서는 module.exports를 의미한다.
  - 브라우저 환경일 때, strict mode라면 undefined가 할당된다.

<br>

## 22.2 함수 호출 방식과 this 바인딩

<br>

자바스크립트에서 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

<br>

1. 일반함수 호출

     -  일반함수 호출이란 함수를 참조하고 있는 식별자에 괄호 연산자를 붙여 호출하는 방식을 의미한다.
        ```js
        // 다음의 경우 모두 일반함수 호출에 해당한다.
        function fn1() {}
        fn1(); 
        const fn2 = function() {};
        fn2();
        const obj = { method: function() {} };
        const fn3 = obj.method;
        fn3();
        // 특히 fn3의 경우 처럼 객체의 메서드를 다른 식별자에 할당만 했음에도 
        // 단지 호출 방식에 따라 일반함수 호출로 분류된다.
        ```
    - 일반 함수 호출 시 기본적으로 this에 전역 객체가 바인딩 된다.
        - 브라우저 환경에서는 window, node 환경에서는 global이 바인딩된다.
    - 단, strict mode일 경우에는 undefined가 할당된다.
        - node REPL에서는 그대로 전역객체가 바인딩된다.

2. 메서드 호출
     - 점 연산자를 통해 객체의 메서드로써 호출되는 함수의 내부에서 this는 점 연산자 앞에 있는 객체에 바인딩된다.
     - 이를 암묵적 바인딩이라고 부른다.

3. 생성자 함수 호출
     - new 연산자와 함께 호출되는 경우 함수는 생성자로써 호출된다.
     - 생성자 함수 내부에서 this는 생성할 객체를 의미한다.
     - 생정자 함수의 동작을 간단히 나타내면 다음과 같다.
        ```js
        function Func() {
          this.a = 1;
        }
        // 위 함수가 생성자 함수로 호출된 경우 다음과 같은 일들이 암묵적으로 일어난다.
        function Func() {
          // 생성될 객체에 this를 바인딩
          this = {};
          // prototype 연결
          // 함수 실행
          this.a = 1;
          // this를 반환
          return this;
        }
        ```

4. Function 객체의 메서드에 의한 호출
     - Function 객체는 this를 명시적으로 바인딩할 수 있는 메서드들을 제공한다.
     - apply, call
       - apply와 call 메서드는 this를 특정 객체에 바인딩 한 채로 함수를 호출한다.
       - 첫번째 인자에 this가 가리킬 객체를 넣고 두번째부터는 함수에게 전달될 인수를 넣는다.
       - apply와 call은 함수의 인수를 전달하는 방식에서만 차이를 갖는다.
          ```js
          function testBinding(...args) {
            console.log(this, args);
          }
          const obj = { a: 1 };
          console.log(getThis.apply(obj, [1, 2, 3])); // { a: 1 }, [1,2,3]
          console.log(getThis.call(obj, 1, 2, 3); // { a: 1 }, [1,2,3]
          // apply는 배열 혹은 유사 배열로 인수를 전달한다.
          // call은 쉼표로 구분해 인수를 각각 전달한다.
          ```
      - bind
        - bind 메서드는 this바인딩이 '고정' 되어있는 함수를 호출한다.
        - bind에 의해 생성된 함수는 일반 함수 호출 시에도 고정된 this바인딩을 보장 받는다.
        - bind 메서드에 전달한 두번째 이후 인수들은 고정된 인수도 여겨진다.
          ```js
          function testBinding(...args) {
            console.log(this, args);
          }
          const obj = { a: 1 };
          const boundObj = testBinding.bind(obj, 1, 2);
          boundObj(3); // { a: 1 } [ 1, 2, 3 ]
          ```
<br>
          
