26 함수의 추가기능

## 1. 함수의 구분

- ES6 이전
  사용 목적에 따라 명확히 구분되지 않는다. 즉, ES6 이전의 모든 함수는 일반함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다. 즉, ES6 이전의 모든 함수는 callable 이면서 constructor다.
- ES6 이후
  ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.

| ES6 함수의 구분 | constructor | prototype | super | arguments |
| --------------- | ----------- | --------- | ----- | --------- |
| 일반 함수       | O           | O         | X     | O         |
| 메서드          | X           | X         | O     | O         |
| 화살표 함수     | X           | X         | X     | X         |



## 2. 메서드

ES6 이전 사양에는 메서드에 대한 명확한 정의가 없었다. ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다. ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다. 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없다.

```js
const obj = {
  x: 1,
  foo() { return this.x; },	// 메서드
  bar: function() { return this.x; }	// 일반 함수
};

console.log(obj.foo());		// 1
console.log(obj.bar());		// 1

// ES6 메서드는 인스턴스 생성 X
new obj.foo();			// ❌ TypeError: obj.foo is not a constructor
new obj.bar();			// bar {}

// ES6 메서드는 prototype 프로퍼티 X
obj.foo.hasOwnProperty('prototype');	// false
obj.bar.hasOwnProperty('prototype');	// true
```

✔ ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

✔ ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.

- super 참조는 내부 슬롯 `[[HomeObject]]`를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 `[[HomeObject]]`를 갖는 ES6 메서드는 `super` 키워드를 사용할 수 있음



## 3. 화살표 함수

화살표 함수(arrow function)는 function 키워드 대신 화살표(=>)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.



### 3.1 화살표 함수 정의

```js
onst arrow = (x, y) => x + y;
```

❗ 화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다.

```js
const arrow = (x, y) => { ... };
                         
const arrow = x => { ... };  

const arrow = () => { ... };
```

✔ 매개변수가 여러 개인 경우 소괄호 () 안에 매개변수를 선언한다. 한 개인 경우 소괄호 ()를 생략할 수 있다.

❗ 매개변수가 없는 경우 소괄호 ()를 생략할 수 없다.



✔ 함수 몸체가 하나의 문으로 구성된다면 {}를 생략할 수 있다.

❗ 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.

```js
const power = x => x * 2;
power(2); // 4

const arrow = () => const x = 1; //SynctaxError
const arrow = () => { const x = 1 };
```

✔ 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호 ()로 감싸 주어야 한다.

```js
const person = (name => ({
  sayHi() { return name; }
}))('Cho');

console.log(person.sayHi()); // Cho
```

✔ 화살표 함수도 즉시 실행 함수로 사용할 수 있다.

```js
(() => {
    // ..
})(); // (O)

```



### 3.2 화살표 함수와 일반 함수의 차이

- 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.
- 중복된 매개변수 이름을 선언할 수 없다.
- 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.



### 3.3 this

> 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위스코프의 this를 그대로 참조한다. 이를 lexical this라 한다. 이는 마치 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.

```js
const foo = () => console.log(this);
foo(); // window
```

✔ 화살표 함수가 전역 함수라면 화살표 함수의 this는 전역 객체를 가리킨다.

❗대표적으로 this 에 바인딩 되는 값

1. 전역 공간의 this : 전역 객체
2. 메소드 호출 시 메소드 내부의 this : 해당 메소드를 호출한 객체
3. 함수 호출 시 함수 내부의 this : **지정되지 않음**(❗️)

📌

```javascript

var cat = {
  name: 'meow',
  foo1: function() {
    const foo2 = function() {
      console.log(this.name);
    }
    foo2();
  }
};

cat.foo1();	// undefined
-----------------------------------
 
 var cat = {
  name: 'meow',
  foo1: function() {
    const foo2 = () => {
      console.log(this.name);
    }
    foo2();
  }
};

cat.foo1();	// meow
```

 function으로 선언한 함수가 메소드로 호출되냐 함수 자체로 호출되냐에 따라 동적으로 this가 바인딩되는 반면, 화살표 함수는 **선언될 시점에서의 상위 스코프**가 this로 바인딩된다.





### 3.4 super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.





### 3.5 arguments

화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다. 따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다.



## 4. Rest 파라미터

> Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 세개의 점 ...을 붙여서 정의한 매개변수를 의미한다. Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

```js
function foo(...rest) {
  console.log(rest); // [1, 2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

✔ 일반 매개변수와 Rest 파라미터는 함께 사용할 수 있다.

 ❗ Rest 파라미터는 반드시 마지막 파라미터이어야 한다.

❗ Rest 파라미터는 단 하나만 선언할 수 있다.

```javascript
function foo(param1, param2, ...rest){
  console.log(param1);	// 1
  console.log(param2);	// 2
  console.log(rest);	// [3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

✔ Rest 파라미터는 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

```javascript
function foo(...rest) {}
console.log(foo.length);	// 0

function bar(x, ...rest) {}
console.log(bar.length);	// 1

function baz(x, y, ...rest) {}
console.log(baz.length);	// 2
```

✔ ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받을 수 있다.



## 5. 매개변수 기본값

```js
function sum(x, y) {
  return x + y;
}

console.log(sum(1));	// NaN

---------------------------
  
function sum (x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1

console.log(sum.length); // 0


```

❗ Rest 파라미터에는 기본값을 지정할 수 없다.

```javascript
function foo(...rest = []) {
  console.log(rest);
}	// SyntaxError: Rest parameter may not have a default initializer
```