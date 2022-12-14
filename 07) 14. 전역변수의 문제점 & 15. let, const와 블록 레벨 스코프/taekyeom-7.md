# 14. 전역변수의 문제점 & 15. let, const와 블록 레벨 스코프

## 14장 전역 변수의 문제점

### 14.1 변수의 생명 주기

#### 14.1.1 지역 변수의 생명 주기

변수는 선언에 의해 생성되고 할당을 통해 값을 갖는다.

변수는 자신이 선언된 위치에서 생성되고 소멸한다. 전역 변수의 생명 주기는 애플리케이션의 생명 주기와 같다. 하지만 함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸한다.

<br>

지역 변수의 생명 주기는 함수의 생명 주기와 일치한다.

일반적으로 함수가 종료하면 함수가 생성한 스코프도 소멸한다. 하지만 누군가가 스코프를 참조하고 있다면 스코프는 해제되지 않고 생존하게 된다. (클로저)

#### 14.1.2 전역 변수의 생명 주기

var 키워드로 선언한 전역 변수의 생명 주기는 전역 객체의 생명 주기와 일치한다.

### 14.2 전역 변수의 문제점

- 암묵적 결합

  전역 변수를 선언한 의도는 전역, 즉 코드 어디서든 참조하고 할당할 수 있는 변수를 사용하겠다는 것이다. 이는 모든 코드가 전역 변수를 참조하고 변경할 수 있는 암묵적 결합을 허용하는 것이다.

- 긴 생명 주기

  전역 변수는 생명 주기가 길다.

- 스코프 체인 상에서 종점에 존재

  전역 변수는 스코프 체인 상에서 종점에 존재한다. 이는 변수를 검색할 때 전역 변수가 가장 마지막에 검색된다는 것을 말한다. 즉 전역 변수의 검색 속도가 가장 느리다.

- 네임스페이스 오염

  자바스크립트의 가장 큰 문제점 중 하나는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다는 것이다. 따라서 다른 파일 내에서 동일한 이름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올 수 있다.

### 14.3 전역 변수의 사용을 억제하는 방법

전역 변수를 사용해야 할 이유를 찾지 못한다면 지역 변수를 사용해야 한다. 변수의 스코프는 좁을수록 좋다.

#### 14.3.1 즉시 실행 함수

모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

#### 14.3.2 네임스페이스 객체

네임스페이스를 분리해서 식별자 충돌을 방지하는 효과는 있으나 네임스페이스 객체 자체가 전역 변수에 할당되므로 그다지 유용해 보이지는 않는다.

#### 14.3.3 모듈 패턴

모듈 패턴은 클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다. 모듈 패턴은 자바스크립트의 강력한 기능인 클로저를 기반으로 동작한다. 모듈 패턴의 특징은 전역 변수의 억제는 물론 캡슐화까지 구현할 수 있다는 것이다.

#### 14.3.4 ES6 모듈

ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다. 따라서 모든 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.

모던 브라우저에서는 ES6 모듈을 사용할 수 있다. script 태그에 type="module" 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한다. 모듈의 파일 확장자는 mjs를 권장한다.

## 15장 let, const 키워드와 블록 레벨 스코프

### 15.1 var 키워드로 선언한 변수의 문제점

#### 15.1.1 변수 중복 선언 허용

var 키워드로 변수는 중복 선언이 가능하다.

<br>

var 키워드로 선언한 변수를 중복 선언하면 초기화문(변수 선언과 동시에 초기값을 할당하는 문) 유무에 따라 다르게 동작한다.

초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하고 초기화문이 없는 변수 선언문은 무시된다. 이때 에러는 발생하지 않는다.

#### 15.1.2 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록내에서 선언해도 모두 전역 변수가 된다.

#### 15.1.3 변수 호이스팅

var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 즉 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

### 15.2 let 키워드

#### 15.2.1 변수 중복 선언 금지

var 키워드로 이름이 동일한 변수를 중복 선언하면 아무런 에러가 발생하지 않는다. 이때 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 재할당되어 변경되는 부작요잉 발생한다.

하지만 let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

#### 15.2.2 블록 레벨 스코프

let 키워드로 선언한 변수는 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인전하는 블록 레벨 스코프를 따른다.

#### 15.2.3 변수 호이스팅

let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러가 발생한다.

<br>

let 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

만약 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 밠생한다. let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없다. 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구각을 일시적 사각지대라고 부른다.

#### 15.2.4 전역 객체와 let

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

### 15.3 const 키워드

#### 15.3.1 선언과 초기화

const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

#### 15.3.2 재할당 금지

const 키워드로 선언한 변수는 재할당이 금지된다.

#### 15.3.3 상수

변수의 상대 개념인 상수는 재할당이 금지된 변수를 말한다.

<br>

const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.

#### 15.3.4 const 키워드와 객체

const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.

const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지 않는다.

### 15.4 var vs let vs const

var와 let const 키워드는 다음과 같이 사용하는 것을 권장한다.

- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드를 사용한다. const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전하다.
