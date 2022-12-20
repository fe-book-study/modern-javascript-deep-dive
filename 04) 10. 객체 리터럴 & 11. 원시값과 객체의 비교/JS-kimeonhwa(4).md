10 객체 리터럴 / 11 원시값과 객체의 비교



## 10장 객체 리터럴

### 1. 객체

> 자바스크립트는 `객체 Object`기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 “모든 것”이 객체이다. 원시 값을 제외한 나머지 값(함수, 배열, 정규표현식등)은 모두 객체이다.
>
> 원시 타입은 단 하나의 값만 나타내지만,
> 객체 타입은(`object type` / `reference type`)은 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조`data structure`이다.
>
> 원시 타입의 값, 즉 원시 값은 변경 불가능한 `immutable value`이지만,
> 객체 타입의 값, 즉 객체는 변경 가능한 값 `mutable value`이다.
>
> 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키`key`와 값`value`으로 구성된다.
>
> 참고로, 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 **메서드`method`**라고 부른다.
>
> > -프로퍼티: 객체의 상태를 나타내는 값(data)
> > -메서드: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)



### 2. 객체 리터럴에 의한 객체 생성

> `c++`이나 `java`같은 클래스 기반 객체지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new 연산자와 함께 생성자(`constructor`)를 호출하여 인스턴스를 생성하는 방식으로 객체를 생성한다.
하지만, 자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법이 있다.

※객체를 생성하는 방법

- 객체 리터럴
- `object` 생성자 함수
- 생성자 함수
- `Object.create` 메서드
- 클래스(ES6)

이 중 가장 일반적이고 간단한 방법은 객체 리터럴이다.
중괄호 `{...}`내에 0개 이상의 프로퍼티를 정의하고, 변수에 할당되는 시점에 자바스크립트 엔진이 해석해 객체를 생성한다.

```js
var person = {
  name: 'Lee',
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  }
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: ƒ}

// 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다.
var empty = {}; // 빈 객체
console.log(typeof empty); // object
```

- 객체 리터럴의 중괄호는 **코드블록**을 의미하지 않는다!!
  객체 리터럴은 값으로 평가되는 표현식이므로, 객체 리터럴의 닫는 중괄호 뒤에 세미콜론`;`을 붙인다.

- 객체 리터럴 외의 객체 생성방식은 모두 함수를 사용한다.



### 3. 프로퍼티

> 객체는 프로퍼티의 집합이며, 프로퍼티는 키`key`와 값`value`으로 구성된다.

```js
var person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: 'Lee',
  // 프로퍼티 키는 age, 프로퍼티 값은 20
  age: 20
};
```

- 프로퍼티를 나열할 때는 쉼표`,`로 구분한다. 일반적으로 마지막 프로퍼티 뒤에는 쉼표`,`를 사용하지 않으나 사용해도 상관없다.

> 프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
> 프로퍼티 값: 자바스크립트에서 사용할 수 있는 모든 값

- 프로퍼티 키는 문자열이므로 따옴표`''`, `""` 로 묶여야 한다. 하지만 [식별자 네이밍 규칙](https://hong-p.github.io/javascript/javascript-deepdive-ch04/#3식별자-네이밍-규칙)을 준수한다면 따옴표는 생략 가능하다.

```js
var person = {
  firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  'last-name': 'Lee'   // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};

console.log(person); // {firstName: "Ung-mo", last-name: "Lee"}
```

- 네이밍 규칙을 지키지 않으면 `SyntaxError` 발생

```js
var person = {
  firstName: 'Ung-mo',
  last-name: 'Lee' // SyntaxError: Unexpected token -
};
```

- 프로퍼티 키를 동적으로 생성 가능하다.

```js
var obj = {};
var key = 'hello';

// ES5: 프로퍼티 키 동적 생성
obj[key] = 'world';
// ES6: 계산된 프로퍼티 이름
// var obj = { [key]: 'world' };

console.log(obj); // {hello: "world"}

```



- 빈 문자열도 가능하나, 키로서의 의미를 갖지 못한다.

```js
var foo = {
  '': ''  // 빈 문자열도 프로퍼티 키로 사용할 수 있다.
};

console.log(foo); // {"": ""}
console.log(foo['']) // '' 접근할때는 []로만 가능
```

- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.

```js
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```

- `var`, `function`과 같은 예약어도 가능하지만 에러가 발생할 여지가 있으므로 권장하지 않는다.

```js
var foo = {
  var: '',
  function: ''
};

console.log(foo); // {var: "", function: ""}
```

- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티 가 덮어쓴다.

```js
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```





### 4. 메서드

> 자바스크립트의 함수는 객체`일급 객체`이기 때문에 값으로 취급되어 프로퍼티 값으로 사용할 수 있다.
> 이러한 경우 메서드`method`라고 부른다.

```js
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () { // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```



### 5. 프로퍼티 접근

> 프로퍼티에 접근하는 방법은 두 가지다.
>
> - 마침표 프로퍼티 접근 연산자`.`를 사용하는 마침표 표기법`dot notation`
> - 대괄호 프로퍼티 접근 연산자`[]`를 사용하는 대괄호 표기법`bracket notation`



```js
var person = {
  name: 'Lee'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); // Lee
```



- 대괄호 프로퍼티 접근 연산자`[]` 내부에 지정하는 프로퍼티 키는 반드시 따옴표(`''`, `""`)로 감싼 문자열이어야 한다. 만약 따옴표로 감싸지 않으면 자바스크립트 엔진은 식별자로 해석한다.

```js
var person = {
  name: 'Lee'
};

console.log(person[name]); // ReferenceError: name is not defined
```



- 객체에 존재하지 않는 프로퍼티에 접근하면 `undefined`를 반환한다. `ReferenceError`가 발생되지 않음!!

```js
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

=> 위 예제에서 `person.last-name`의 실행결과가 브라우저와 Node.js 환경이 다른 이유는??
`person.last-name`을 평가할 때 자바스크립트 엔진은 `person.last`를 먼저 평가하는데,
브라우저 환경에는 `window`객체에 `name`의 프로퍼티가 암묵적으로 전재하고 기본값은 빈 문자열이다.
이에 반해 Node.js 환경에서는 전역변수 name이 존재하지 않기 대문에 `ReferenceError`가 발생한다.



### 6. 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```js
var person = {
  name: 'Lee'
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person);  // {name: "Kim"}
```





### 7. 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.

```js
var person = {
  name: 'Lee'
};

// person 객체에는 age 프로퍼티가 존재하지 않는다.
// 따라서 person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```



### 8. 프로퍼티 삭제

- `delete`연산자를 활용해 객체의 프로퍼티를 삭제한다. 이때 `delete`연산자의 피연산자는 프로퍼티 값에 접글할 수 있는 표현식이어야 한다. 만약 존재하지 않는 프로퍼티인 경우 아무런 에러 없이 무시된다.

```js
var person = {
  name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// person 객체에 age 프로퍼티가 존재한다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.address;

console.log(person); // {name: "Lee"}


// 프로퍼티를 지울 수 없게 생성하기
Object.defineProperty(person, 'age', {value:20, configurable:false}) 
delete person.age // false
console.log(person); // {name: "Lee", age: 20}

```

- 전역 객체로 생성하는 경우 특이한 점

```js
// window 객체에 프로퍼티를 추가하는 경우
window.a = 10
console.log(window.a) // 10

delete window.a // true
console.log(window.a) // undefined


// var 키워드로 생성하는 경우
var a = 10
console.log(window.a) // 10

delete window.a // false
console.log(window.a) // 10

```

###  Inyong Hong

For tomorrow better than today!

Follow

# 모던 자바스크립트 딥 다이브 10장 객체 리터럴

 Updated: November 14, 2021

 On this page[10장 객체 리터럴](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#10장-객체-리터럴)[1.객체란?](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#1객체란)[2.객체 리터럴에 의한 객체 생성](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#2객체-리터럴에-의한-객체-생성)[3.프로퍼티](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#3프로퍼티)[4.메서드](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#4메서드)[5.프로퍼티 접근](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#5프로퍼티-접근)[6.프로퍼티 값 갱신](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#6프로퍼티-값-갱신)[7.프로퍼티 동적 생성](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#7프로퍼티-동적-생성)[8.프로퍼티 삭제](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#8프로퍼티-삭제)[9.ES6에서 추가된 객체 리터럴의 확장 기능](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#9es6에서-추가된-객체-리터럴의-확장-기능)[9.1 프로퍼티 축약 표현](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#91-프로퍼티-축약-표현)[9.2 계산된 프로퍼티 이름](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#92-계산된-프로퍼티-이름)[9.3 메서드 축약 표현](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#93-메서드-축약-표현)



# 10장 객체 리터럴[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#10장-객체-리터럴)

## 1.객체란?[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#1객체란)

자바스크립트는 `객체 Object`기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 “모든 것”이 객체이다. 원시 값을 제외한 나머지 값(함수, 배열, 정규표현식등)은 모두 객체이다.



원시 타입은 단 하나의 값만 나타내지만,
객체 타입은(`object type` / `reference type`)은 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조`data structure`이다.



원시 타입의 값, 즉 원시 값은 변경 불가능한 `immutable value`이지만,
객체 타입의 값, 즉 객체는 변경 가능한 값 `mutable value`이다.



객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키`key`와 값`value`으로 구성된다.



참고로, 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 **메서드`method`**라고 부른다.

> -프로퍼티: 객체의 상태를 나타내는 값(data)
> -메서드: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

## 2.객체 리터럴에 의한 객체 생성[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#2객체-리터럴에-의한-객체-생성)

`c++`이나 `java`같은 클래스 기반 객체지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new 연산자와 함께 생성자(`constructor`)를 호출하여 인스턴스를 생성하는 방식으로 객체를 생성한다.
하지만, 자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법이 있다.

※객체를 생성하는 방법

- 객체 리터럴
- `object` 생성자 함수
- 생성자 함수
- `Object.create` 메서드
- 클래스(ES6)

이 중 가장 일반적이고 간단한 방법은 객체 리터럴이다.
중괄호 `{...}`내에 0개 이상의 프로퍼티를 정의하고, 변수에 할당되는 시점에 자바스크립트 엔진이 해석해 객체를 생성한다.

```
var person = {
  name: 'Lee',
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  }
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: ƒ}

// 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다.
var empty = {}; // 빈 객체
console.log(typeof empty); // object
```

객체 리터럴의 중괄호는 **코드블록**을 의미하지 않는다!!
객체 리터럴은 값으로 평가되는 표현식이므로, 객체 리터럴의 닫는 중괄호 뒤에 세미콜론`;`을 붙인다.



객체 리터럴 외의 객체 생성방식은 모두 함수를 사용한다.

## 3.프로퍼티[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#3프로퍼티)

객체는 프로퍼티의 집합이며, 프로퍼티는 키`key`와 값`value`으로 구성된다.

```
var person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: 'Lee',
  // 프로퍼티 키는 age, 프로퍼티 값은 20
  age: 20
};
```

프로퍼티를 나열할 때는 쉼표`,`로 구분한다. 일반적으로 마지막 프로퍼티 뒤에는 쉼표`,`를 사용하지 않으나 사용해도 상관없다.

> 프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
> 프로퍼티 값: 자바스크립트에서 사용할 수 있는 모든 값

프로퍼티 키는 문자열이므로 따옴표`''`, `""` 로 묶여야 한다. 하지만 [식별자 네이밍 규칙](https://hong-p.github.io/javascript/javascript-deepdive-ch04/#3식별자-네이밍-규칙)을 준수한다면 따옴표는 생략 가능하다.

```
var person = {
  firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  'last-name': 'Lee'   // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};

console.log(person); // {firstName: "Ung-mo", last-name: "Lee"}
```



네이밍 규칙을 지키지 않으면 `SyntaxError` 발생

```
var person = {
  firstName: 'Ung-mo',
  last-name: 'Lee' // SyntaxError: Unexpected token -
};
```



프로퍼티 키를 동적으로 생성 가능하다.

```
var obj = {};
var key = 'hello';

// ES5: 프로퍼티 키 동적 생성
obj[key] = 'world';
// ES6: 계산된 프로퍼티 이름
// var obj = { [key]: 'world' };

console.log(obj); // {hello: "world"}
```



빈 문자열도 가능하나, 키로서의 의미를 갖지 못한다.

```
var foo = {
  '': ''  // 빈 문자열도 프로퍼티 키로 사용할 수 있다.
};

console.log(foo); // {"": ""}
console.log(foo['']) // '' 접근할때는 []로만 가능
```



프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.

```
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```



`var`, `function`과 같은 예약어도 가능하지만 에러가 발생할 여지가 있으므로 권장하지 않는다.

```
var foo = {
  var: '',
  function: ''
};

console.log(foo); // {var: "", function: ""}
```



이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티 가 덮어쓴다.

```
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```

## 4.메서드[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#4메서드)

자바스크립트의 함수는 객체`일급 객체`이기 때문에 값으로 취급되어 프로퍼티 값으로 사용할 수 있다.
이러한 경우 메서드`method`라고 부른다.

```
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () { // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```

## 5.프로퍼티 접근[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#5프로퍼티-접근)

프로퍼티에 접근하는 방법은 두 가지다.

- 마침표 프로퍼티 접근 연산자`.`를 사용하는 마침표 표기법`dot notation`
- 대괄호 프로퍼티 접근 연산자`[]`를 사용하는 대괄호 표기법`bracket notation`

```
var person = {
  name: 'Lee'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); // Lee
```

대괄호 프로퍼티 접근 연산자`[]` 내부에 지정하는 프로퍼티 키는 반드시 따옴표(`''`, `""`)로 감싼 문자열이어야 한다. 만약 따옴표로 감싸지 않으면 자바스크립트 엔진은 식별자로 해석한다.

```
var person = {
  name: 'Lee'
};

console.log(person[name]); // ReferenceError: name is not defined
```



객체에 존재하지 않는 프로퍼티에 접근하면 `undefined`를 반환한다. `ReferenceError`가 발생되지 않음!!

```
var person = {
  name: 'Lee'
};

console.log(person.age); // undefined
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

위 예제에서 `person.last-name`의 실행결과가 브라우저와 Node.js 환경이 다른 이유는??
`person.last-name`을 평가할 때 자바스크립트 엔진은 `person.last`를 먼저 평가하는데,
브라우저 환경에는 `window`객체에 `name`의 프로퍼티가 암묵적으로 전재하고 기본값은 빈 문자열이다.
이에 반해 Node.js 환경에서는 전역변수 name이 존재하지 않기 대문에 `ReferenceError`가 발생한다.

## 6.프로퍼티 값 갱신[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#6프로퍼티-값-갱신)

이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```
var person = {
  name: 'Lee'
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person);  // {name: "Kim"}
```

## 7.프로퍼티 동적 생성[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#7프로퍼티-동적-생성)

존재하지 않는 프로퍼티에 값을 할당하면 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.

```
var person = {
  name: 'Lee'
};

// person 객체에는 age 프로퍼티가 존재하지 않는다.
// 따라서 person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

## 8.프로퍼티 삭제[Permalink](https://hong-p.github.io/javascript/javascript-deepdive-ch10/#8프로퍼티-삭제)

`delete`연산자를 활용해 객체의 프로퍼티를 삭제한다. 이때 `delete`연산자의 피연산자는 프로퍼티 값에 접글할 수 있는 표현식이어야 한다. 만약 존재하지 않는 프로퍼티인 경우 아무런 에러 없이 무시된다.

```
var person = {
  name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// person 객체에 age 프로퍼티가 존재한다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.address;

console.log(person); // {name: "Lee"}


// 프로퍼티를 지울 수 없게 생성하기
Object.defineProperty(person, 'age', {value:20, configurable:false}) 
delete person.age // false
console.log(person); // {name: "Lee", age: 20}
```

전역 객체로 생성하는 경우 특이한 점

```
// window 객체에 프로퍼티를 추가하는 경우
window.a = 10
console.log(window.a) // 10

delete window.a // true
console.log(window.a) // undefined


// var 키워드로 생성하는 경우
var a = 10
console.log(window.a) // 10

delete window.a // false
console.log(window.a) // 10
```



- 위에서 확인한 `Object.defineProperty()` 함수로 `configurable:false`를 준것과 동일하게 프로퍼티를 생성하는 듯!





### 9. ES6 에서 추가된 객체 리터럴의 확장 기능

> ES6에서 추가된 객체 리터럴 확장기능
>
> - 프로퍼티 축약 표현
> - 계산된 프로퍼티 이름
> - 메서드 축약 표현

#### 프로퍼티 축약 표현

- 변수 이름과 프로퍼티 키가 동일한 이름인 경우 프로퍼티 키를 생략`property shorthand`할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.

```js
// ES5
var x = 1, y = 2;

var obj = {
  x: x,
  y: y
};

console.log(obj); // {x: 1, y: 2}


// ES6
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```



#### 계산된 프로퍼티 이름

- 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성 할 수 있다. 단 대괄호`[]`로 묶어 사용해야 한다. 이를 `computed property name`이라고 한다.
- ES5에서는 객체 리터럴 외부에서 대괄호`[]` 표기법을 사용해야 하고,
  ES6에서는 객체 리터럴 내부에서도 생성할 수 있다.

```js
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}


// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};
```



#### 메서드 축약 표현

- ES5에서는 아래와 같이 콜론`:`과 `function`키워드로 객체 리터럴 내부에서 생성하지만,
  ES6에서는 콜론`:`과 `function` 키워드를 생략한 축약 표현을 사용할 수 있다.

```js
// ES5
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee


// ES6
const obj = {
  name: 'Lee',
  // 메서드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```



```js
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function() { return this.x; }
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1


new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}


// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // -> true
```





## 11. 원시값과 객체의 비교



### 1. 원시값

#### 변경 불가능한 값

> 원시 타입`primitive type`의 값은 변경 불가능한 값`immutable value`이다. 한번 생성된 원시값은 읽기 전용`read only`값으로서 변경할 수 없다.
> 변경 불가능 하다는 것은 변수가 아니라 값에 대한 진술이다.
> 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있다.
> 변수 값을 변경하기 위해 원시 값을 재할당하면 새로운 메모리 공간을 확보하고 재할당한 값을 저장한 후, 변수가 참조하던 메모리 공간의 주소를 변경한다. 값의 이러한 특성을 **불변성**`immutability`라고 한다.

#### 문자열과 불변성

> *ECMAScript 사양에 따르면 원시 타입의 크기를 아래와 같이 규정하고 있다.*
> *문자열 타입: 2바이트*
> *숫자 타입: 8바이트*
> *이외의 원시 타입은 규정되어 있지 않아 브라우저 제조사의 구현에 따라 다를 수 있다.*

```js
// 문자열은 0개 이상의 문자들로 이뤄진 집합이다.
var str1 = '';      // 0개의 문자로 이뤄진 문자열(빈 문자열)
var str2 = 'Hello'; // 5개의 문자로 이뤄진 문자열
```



```js
var str = 'Hello';
str = 'world';
```

- 첫 번째 문이 실행되면 문자열 `'Hello'`가 생성되고 식별자 `str`은 문자열 `'Hello'`가 저장된 메모리 공간의 첫 번째 메모리 셀 주소를 가리킨다.
  그리고 두 번째 문이 실행되면 이전에 생성된 문자열 `Hello`를 수정하는 것이 아니라 새로운 문자열 `world`를 메모리에 생성하고 식별자 `str`은 이것을 가리킨다. 이때 문자열 `'Hello'`와 `'world'`는 모두 메모리에 존재한다.
  식별자 `str`은 문자열 `'Hello'`를 가리키고 있다가 문자열 `'world'`를 가키리도록 변경되었을 뿐이다.



- *유사 배열 객체*
유사 배열 객체란 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 `length` 프로퍼티를 갖는 객체를 말한다.
문자열은 마치 배열처럼 인덱스를 통해 각 문자에 접글할 수 있으며, `length` 프로퍼티를 갖기 때문에 유사 배열 객체이고 `for`문으로 순회할 수 도 있다.

```js
var str = 'string';

// 문자열은 유사 배열이므로 배열과 유사하게 인덱스를 사용해 각 문자에 접근할 수 있다.
console.log(str[0]); // s

// 원시 값인 문자열이 객체처럼 동작한다.
console.log(str.length); // 6
console.log(str.toUpperCase()); // STRING
```

- 원시값인 문자열이 객체처럼 동작한다는 것은 래퍼 객체(`wrapper object`)로 자동 변환되기 때문인데 아래를 참고하자.



- *원시값과 래퍼 객체*
마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 준다. 즉, 원시값을 객체 처럼 사용하면 자바스크립트 엔진이 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출한다. 작업을 마친 후 다시 원시값으로 되돌린다.

- 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 `래퍼 객체(wrapper object)`라고 한다.

```js
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```



```js
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

- 위 예제에서 `str.toUpperCase()`이 부분을 풀어서 보면 아래와 같이 temp객체를 만들어서
  `String` 생성자 함수로 임시 객체를 만들고 `String`객체가 가지고 있는 `toUpperCase()`함수를 사용해 그 결과를 리턴해 주는식으로 동작한다.

```js
const str = 'hello';
// str.toUpperCase()
const temp = new String(str);
temp.toUpperCase()
```



#### 값에 의한 전달

> 변수`copy`에 원시 값을 갖는 변수`score`를 할당하면 할당받는 변수`copy`에는 할당되는 변수`score`의 원시 값이 복사되어 전달된다.

```js
var score = 80;
var copy = score;

console.log(score); // 80
console.log(copy);  // 80
```

```js
var score = 80;

// copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
var copy = score;

console.log(score, copy); // 80  80
console.log(score === copy); // true

```

- `score`변수와 `copy`변수의 값 80은 다른 메모리 공간에 저장된 별개의 값이다.

```js
var score = 80;

// copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
var copy = score;

console.log(score, copy);    // 80  80
console.log(score === copy); // true

// score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값이다.
// 따라서 score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않는다.
score = 100;

console.log(score, copy);    // 100  80
console.log(score === copy); // false
```

- `score`변수의 값을 100으로 변경하더라도 `copy`변수에는 어떠한 영향도 주지 않는다.



### 2. 객체

> 자바스크립트 객체는 프로퍼티의 개수가 정해져 있지 않고, 동적으로 추가되고 삭제 가능하다. 또한 프로퍼티의 값에도 제약이 없다.
> 원시 값과 같이 확보해야 할 메모리 공간의 크기를 사전에 정해 둘 수 없다.
>
> > *※객체의 관리 방식*
> > `java`, `c++`같은 클래스 기반 객체지향 프로그래밍 언어는 사전에 정의된 클래스를 기반으로 객체를 생성한다. 객체가 생성된 이후에는 프로퍼티를 삭제하거나 추가할 수 없다.
> > 하지만 자바스크립트는 클래스 없이 객체를 생성할수 있으며, 생성된 이후에도 동적으로 프로퍼티와 메서드를 추가할 수 있다.
> > 이는 매우 편리하지만 성능 면에서는 이론적으로 클래스 기반 언어 보다 생성과 접근에 비용이 더 많이 드는 비효율적인 방식이다.
> > v8 자바스크립트 엔진에서는 히든 클래스`hidden class`라는 방식을 사용해 `c++`객체의 프로퍼티에 접근하는 정도의 성능을 보장한다.



#### 변경 가능한 값

- 객체 타입의 값, 즉 객체는 변경 가능한 값`mutable vlaue`이다.

```js
var person = {
  name: 'Lee'
};
```

- `person` 변수는 참조 값(`reference value`)에 접근할 수 있는 메모리 주소를 기억하고 있다.
- 원시 값을 할당한 변수를 참조하면 메모리에 저장되어 있는 원시 값에 접근한다.
  하지만 객체를 할당한 변수`person`를 참조하면 메모리에 저장되어 있는 참조 값(`0x00...1`)을 통해 실제 객체`{name: 'LEE'}`에 접근한다.

```js
// 할당이 이뤄지는 시점에 객체 리터럴이 해석되고, 그 결과 객체가 생성된다.
var person = {
  name: 'Lee'
};

// person 변수에 저장되어 있는 참조값으로 실제 객체에 접근해서 그 객체를 반환한다.
console.log(person); // {name: "Lee"}

```



```js
var person = {
  name: 'Lee'
};

// 프로퍼티 값 갱신
person.name = 'Kim';

// 프로퍼티 동적 생성
person.address = 'Seoul';

console.log(person); // {name: "Kim", address: "Seoul"}
```

- 객체는 변경 가능한 값이므로 메모리에 저장된 객체를 직접 수정할 수 있다.
  재할당이 필요없다.
  프로퍼티를 동적으로 추가할 수 있고, 갱신할 수 있다.

- **객체를 복사해 성성하는 비용을 절약하여 성능을 향상시키기 위해 객체는 변경 가능한 값으로 설계되어 있다.**
  하지만 이러한 구조적 단점에 따른 부작용이 있다. 그것은 원시 값과는 다르게 **여러개의 식별자가 하나의 객체를 공유할 수 있다**는 것이다.

*※ 얕은 복사`shallow copy`와 깊은 복사`deep copy`*
얕은 복사는 객체의 한 단계까지만 복사하는 것을 말하고,
깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사하는 것을 말한다.

```js
const o = { x: { y: 1 } };

// 얕은 복사
const c1 = { ...o }; // 35장 "스프레드 문법" 참고
console.log(c1 === o); // false
console.log(c1.x === o.x); // true

// lodash의 cloneDeep을 사용한 깊은 복사
// "npm install lodash"로 lodash를 설치한 후, Node.js 환경에서 실행
const _ = require('lodash');
// 깊은 복사
const c2 = _.cloneDeep(o);
console.log(c2 === o); // false
console.log(c2.x === o.x); // false
```

- 브라우저 환경에서는 `JSON` 객체를 활용해서 깊은 복사`deep copy`를 할 수 있다.

```js
const o = { x: { y: 1 } };

// 얕은 복사
const c1 = { ...o }; // 35장 "스프레드 문법" 참고
console.log(c1 === o); // false
console.log(c1.x === o.x); // true

// 깊은 복사
const c2 = JSON.parse(JSON.stringify(c1))
console.log(c2 === o); // false
console.log(c2.x === o.x); // false

```

- 얕은 복사를 했을 때 `x`프로퍼티의 값을 확인해 보면 `true`가 나오는데 이러한 이유는 값을 복사한것이 아니고 참조 값만 복사해 같은 주소를 참조하고 있기 때문이다.
  `JSON` 객체를 사용해 복사하는 경우 원시값 처럼 완전한 복사본을 만들기 때문에 비교시 `false`가 반환된다.



#### 참조에 의한 전달

> 여러개의 식별자가 하나의 객체를 공유할 수 있다는 것은 객체를 가리키는 변수`person`를 다른 변수`copy`에 할당하면 원본의 참조 값이 복사되어 전달된다는 것이다.

```js
var person = {
  name: 'Lee'
};

// 참조값을 복사(얕은 복사)
var copy = person;
```

- 즉 `person`과 `copy`의 메모리 주소는 다르지만 그 주소 안에는 동일한 참조 값(주소)를 갖는다. 다시 말해 `person`과 `copy`는 모두 동일한 객체를 가리킨다.
  그래서 둘 중 하나를 변경(변수에 새로운 객체를 재 할당하는 것이 아니라, 프로퍼티 값을 변경하거나, 추가, 삭제)하면 서로 영향을 주고 받는다.

```js
var person = {
  name: 'Lee'
};

// 참조값을 복사(얕은 복사). copy와 person은 동일한 참조값을 갖는다.
var copy = person;

// copy와 person은 동일한 객체를 참조한다.
console.log(copy === person); // true

// copy를 통해 객체를 변경한다.
copy.name = 'Kim';

// person을 통해 객체를 변경한다.
person.address = 'Seoul';

// copy와 person은 동일한 객체를 가리킨다.
// 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다.
console.log(person); // {name: "Kim", address: "Seoul"}
console.log(copy);   // {name: "Kim", address: "Seoul"}

```



=> `값에 의한 전달`과 `참조에 의한 전달`은 식별자가 기억하는 **메모리 공간에 저장되어 있는 값을 복사해서 전달**한다는 면에서 동일하다.
다만, 식별자가 기억하는 메모리 공간, 즉 **변수에 저장되어 있는 값이 원시 값이냐, 참조 값이냐의 차이**로 동작이 다른 것이다.