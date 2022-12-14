# 23. 실행컨텍스트 & 13. 스코프

## 13. 실행컨텍스트 

실행 컨텍스트(execution context) 자바스크립트의 동작 원리를 담고 있는 핵심 개념이다.
스코프를 기반으로 식별자와 식별자에 바인딩된 값을 관리하는 방식과 호이스팅이 발생하는 이유, 클로저의 동작 방식, 그리고 태스트 큐와 함께 동작하는 이벤트 핸들러와 비동기 처리의 동작 방식을 이해할 수 있다.

### 13.1 소스코드의 타입

ECMAScript 사양은 소스코드를 4가지 타입으로 구분한다. 4가지 타입의 소스코드는 실행 컨텍스트를 생성한다.

1.전역 코드(global code)
전역 코드는 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프를 생성해야 한다. 그리고 var키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수를 전역 객체의 프로퍼티와 메서드로 바인딩하여 연결 한다. 이를 위해 전역 코드가 평가되면 전역 실행 컨텍스트가 생성된다.

2.함수 코드(function code)
함수코드는 지역 스코프를 생성하고 지역 변수, 매개변수, arguments객체를 관리해야 한다. 그리고 생성한 지역 스코프를 전역 스코프에서 시작하는 스코프 체인의 일원으로 연결해야 한다. 이를 위해 함수 코드가 평가되면 함수 실행 컨텍스트가 생성된다.

3.eval 코드(eval code)
eval코드는 strict mode에서 자신만의 독자적인 스코프를 생성한다. 이를 위해 eval코드가 평가되면 eval실행 컨텍스트가 생성된다.

4.모듈 코드(module code)
모듈 코드는 모듈별로 독립적인 모듈 스코프를 생성한다. 이를 위해 모듈 코드가 평가되면 모듈 실행 컨텍스트가 생성된다.

### 23.2 소스코드의 평가와 실행

자바스크립트 엔진은 소스코드의 평가, 소스코드의 실행 이라는 2개의 과정으로 나누어 처리한다. 

```javascript
var x;
x = 1;
```

1. 소스코드 평가
- 실행 컨텍스트를 생성.
- 변수, 함수등의 선언문 실행
- 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록

2. 소스코드 실행
- 선언문을 제외한 소스코드가 순차적으로 실행(런타임)
- 실행에 필요한 정보(변수나 함수의 참조)를 실행 컨텍스트가 관리하는 스코프에서 검색해서 취득
- 변수 값의 변경등 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록

### 23.3 실행컨텍스트의 역할

```javascript
// 전역 변수 선언
const x = 1;
const y = 2;

// 함수 정의
function foo(a) {
  // 지역 변수 선언
  const x = 10;
  const y = 20;

  // 메서드 호출
  console.log(a + x + y); // 130
}

// 함수 호출
foo(100);

// 메서드 호출
console.log(x + y); // 3
```

1. 전역 코드 평가
- 전역 코드를 실행하기 전에 전역 코드 평가 과정을 거쳐 준비를 한다.
- 선언문만 먼저 실행한다.const x; const y;
- 생성된 전역 변수와 전역 함수가 실행 컨텍스트가 관리하는 전역 스코프에 등록된다.
- 이때 var키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수는 전역 객체의 프로퍼티와 메서드가 된다.(브라우저는 window객체의 프로퍼티와 메서드가 된다.)

2. 전역 코드 실행
- 평가 과정이 끝나면 런타임이 시작되어 코드가 순차적으로 실행된다.
- 전역 변수에 값이 할당되고 함수가 호출된다.x = 1; y = 1;
- 함수가 호출되면 전역코드의 실행을 중단하고 코드 실행 순서를 변경하여 함수 내부로 진입한다.foo(100);

3. 함수 코드 평가
- 함수 내부로 진입하면 내부의 문들을 실행하기 전에 함수 코드 평가 과정을 거치며 준비한다.
- 매개변수(a)와 지역 변수 선언문이const x; const y; 먼저 실행되고 실행 컨텍스트가 관리하는 지역 스코프에 등록된다.
- arguments객체가 생성되어 지역 스코프에 등록되고, this바인딩도 결정된다.

4. 함수 코드 실행
- 평가 과정이 끝나면 런타임이 시작되어 코드가 순차적으로 실행된다.
- 매개변수와 지역 변수에 값이 할당x = 10; y = 10;되고 console.log(a + x + y);메서드가 호출된다.
- console식별자는 스코프 체인을 통해 검색한다.(현재 스코프에 없으면 상위 스코프로 올라가서 검색)

위와 같이 동작을 관리하기 위해 실행 컨텍스트가 필요하다.

> 📍 실행 컨텍스트의 필요성
> - 선언에 의해 생성된 모든 식별자를 스코프를 구분하여 등록하고 상태 변화를 지속적으로 관리할 수 있어야 한다.
> - 스코프는 중첩 관계에 의해 스코프 체인을 형성해야 한다. 스코프 체인을 통해 상위 스코프로 이동해 검색할 수 있어야 한다.
> - 현재 실행 중인 코드의 실행 순서를 변경할 수 있어야 한다. 다시 되돌아 가는것도 가능해야 한다.

### 23.4 실행 컨텍스트 스택

소스코드의 타입별로(전역 코드나 함수 코드) 자바스크립트 엔진은 코드를 평가하여 실행 컨텍스트를 생성하는데, 실행 컨텍스트를 스택 자료구조로 관리한다. 이를 실행 컨텍스트 스택이라고 한다.
실행 컨텍스트 스택은 코드의 실행 순서를 관리한다.

```javascript
const x = 1;

function foo () {
  const y = 2;

  function bar () {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```

1. 전역 코드의 평가와 실행
- 전역 코드를 평가하여 전역 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시
- 이때 전역 변수 x와 전역 함수 foo는 전역 실행 컨텍스트에 등록
- 이후 전역 코드가 실행되어 전역 변수 x에 값이 할당되고 전역 함수 foo가 호출

2. foo함수 코드의 평가와 실행
- foo함수가 호출되면 전역 코드의 실행은 중단되고 제어권이 foo함수 내부로 이동
- foo함수 내부의 함수 코드를 평가하여 foo함수 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시
- 이때 지역변수 y와 중첩함수 bar가 foo함수 실행 컨텍스트에 등록
- 이후 foo함수 코드가 실행되어 지역변수y에 값이 할당되고 중첩함수 bar가 호출

3. bar함수 코드의 평가와 실행
- foo함수 코드의 실행은 일시 중단되고 제어권이 bar함수 내부로 이동
- bar함수 코드를 평가하여 bar함수 시행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시
- 이때 bar함수의 지역변수z가 bar함수 실행 컨텍스트에 등록
- 이후 bar함수가 실행되어 z에 값이 할당되고 console.log메서드를 호출 한 이후 bar함수는 종료

4. foo함수코드로 복귀
- bar함수가 종료되면 제어권은 다시 foo함수로 이동
- bar함수 실행 컨텍스트를 스택에서 팝하여 제거
- foo함수 종료

5. 전역 코드로 복귀
- foo함수가 종료되면 제어권은 다시 전역 코드로 이동
- 더이상 실행할 전역코드가 없어 전역 실행 컨텍스트도 스택에서 팝하여 제거

> 📍 실행 중인 실행 컨텍스트(running execution context)
> 실행 컨텍스트의 최상위에 존재하는 실행 컨텍스트, 현재 실행 중인 코드의 실행 컨텍스트

### 23.5 렉시컬 환경

렉시컬 환경(Lexical Environment)은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다.
렉시컬 환경은 스코프와 식별자를 관리한다.

### 23.6.실행 컨텍스트의 생성과 식별자 검색 과정

1. 전역 객체 생성
전역 객체는 전역 코드가 평가되기 이전에 생성된다.
2. 전역 코드 평가
3. 전역 코드 실행

전역 코드가 순차적으로 실행 된다.
변수 할당문이 실행되어 전역 변수 x, y에 값이 할당된다.

4. foo 함수 코드 평가

### 23.7.실행 컨텍스트와 블록 레벨 스코프

var키워드로 선언된 변수는 오로지 함수의 코드 블록만 지역 스코프로 인정하는 함수 레벨 스코프를 따른다.
하지만 let,const키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```javascript
let x = 1;

if (true) {
  let x = 10;
  console.log(x); // 10
}

console.log(x); // 1
```

위 예제를 보면 if문의 코드 블록 내에서 let키워드로 변수가 선언되었다. 따라서 if문의 코드 블록이 실행되면 if문의 코드 블록을 위한 블록 레벨 스코프를 생성해야 한다.
이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존의 전역 렉시컬 환경을 교체한다.


## 13. 스코프 

### 13.1 스코프란?

스코프란 식별자가 유효한 범위라는 뜻으로,
식별자 자신이 선언된 위치에 의해 다른 코드가 자신을 참조할 수 있는 유효 범위를 말한다.

```javascript
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x); // local
}

foo();

console.log(x); // global
```

전역에 선언된 var x = 'global';과 foo함수 내에 선언된 var x = 'local';은 식별자 이름은 같지만 스코프는 다르다.

이 때 식별자 결정을 해야하는데, 이름이 같은 두 개의 변수 중 어떤 변수를 참조해야 할 것인지를 결정하게 된다. 

따라서 스코프란 자바스크립트 엔진이 **식별자를 검색할 때 사용하는 규칙**이라고도 할 수 있다.

식별자는 유일해야 하며 식별자인 변수 이름은 중복될 수 없다. 하지만 스코프 내에서 식별자는 유일해야 하지만 다른 스코프에는 같은 이름의 식별자를 사용할 수 있다.

> 📍 var 키워드로 선언한 변수의 중복 선언
> var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언이 허용된다.
> 이는 의도치 않게 변수 값이 재할당되어 변경되는 부작용을 발생시킨다. 
> 반면 let이나 const는 중복 선언을 허용하지 않는다.

### 13.2 스코프의 종류

코드는 전역과 지역으로 구분할 수 있다. 

1. 전역과 전역 스코프

전역이란 코드의 가장 바깥 영역
전역은 전역 스코프global scope를 만든다.
전역 변수global variable는 어디서든 참조할 수 있다.

2. 지역과 지역 스코프

지역이란 함수 몸체 내부를 말한다.(자바스크립트는 함수 스코프를 따르기 때문)
지역은 지역 스코프를(local scope) 만든다.
지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.

### 13.3 스코프의 체인

함수는 중첩될 수 있어 함수의 지역 스코프도 중첩될 수 있다.
함수가 중첩되면 스코프도 계층적인 구조를 갖는다.

변수를 참조할 때 자바스크립트 엔진은 소코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작해 상위 스코프의 방향으로 이동하며 선언된 변수를 검색identifier resolution 한다.

스코프 체인은 실행 컨텍스트의 렉시컬 환경을 단방향으로 연결한 것이다. 자세한 사항은 렉시컬 환경을 참고하자.

```javascript
// 전역 함수
function foo() {
  console.log('global function foo');
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log('local function foo');
  }

  foo(); // ①
}

bar();
```

①에서 호출한 foo함수는 전역에 선언한 foo함수가 아니라 bar함수 내부에 선언한 foo함수를 호출 하는 것이다.
①에서 foo();를 호출하면 현재 실행 중인 실행컨텍스트(bar함수)의 렉시컬 환경에서 foo식별자를 먼저 검색한다. 있으면 그 식별자를 참조하고, 없는경우 외부 렉시컬환경(상위 렉시컬 환경)으로 가서 찾기를 반복한다.
이를 식별자를 검색하는 규칙이라고 표현한다.