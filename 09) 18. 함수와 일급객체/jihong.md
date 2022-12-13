## 18. 함수와 일급객체

### 일급 객체

 📍 일급 객체의 조건
  무명 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  변수나 자료구조(객체, 배열)등에 저장할 수 있다.
  함수의 매개변수에 전달할 수 있다.
  함수의 반환값으로 사용할 수 있다.


```javascript
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에게 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- 함수를 객체와 동일하게 사용할 수 있다.
- 객체는 값이므로 함수도 값으로 동일하게 취급할 수 있다.
- 변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문에 리터럴로 정의할 수 있다.
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.
- 반면, 일반 객체는 호출할 수 없지만, 함수 객체는 호출할 수 있다.

### 함수 객체의 프로퍼티

함수도 객체이기 때문에 프로퍼티를 가질 수 있다.
console.dir로 함수 내부를 볼 수 있다.
```javascript
function square(number) {
  return number * number;
}

console.dir(square);
```
Object.getOwnPropertyDescriptors메서드로 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트를 확인할 수 있다.

```javascript
function square(number) {
  return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));
/*
{
  length: {value: 1, writable: false, enumerable: false, configurable: true},
  name: {value: "square", writable: false, enumerable: false, configurable: true},
  arguments: {value: null, writable: false, enumerable: false, configurable: false},
  caller: {value: null, writable: false, enumerable: false, configurable: false},
  prototype: {value: {...}, writable: true, enumerable: false, configurable: false}
}
*/

// __proto__는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptor(square, '__proto__')); // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

__proto__는 접근자 프로퍼티이며, 함수 객체 고유의 프로퍼티가 아니라 Object.prototype객체의 프로퍼티를 상속받은 것이다.

1. arguments 프로퍼티

함수 객체의 arguments프로퍼티 값은 arguments객체다.
arguments객체는 함수 호출 시 전달된 인수(arguments)들의 정보를 담고 있는 순회 가능한(iterable)유사 배열 객체이다.
함수 내부에서 지역 변수처럼 사용된다.

arguments프로퍼티는 ES3부터 폐지되어 일부 브라우저에서만 지원하고 있다.
따라서 Function.arguments와 같은 사용법은 권장지 않음.

자바스크립트는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다. 따라서 함수 호출시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않는다.
선언된 매개변수의 개수보다 인수를 적게 전달한경우 undefined으로 초기화된 상태로 유지된다.
초과된 인수는 무시된다.(but, arguments객체에는 보관됨)

2. caller 프로퍼티

ECMAScript 사양에 포함되지 않은 비표준 프로퍼티이다.
함수 객체의 caller프로퍼티는 함수 자신을 호출한 함수를 가리킨다.
중요하지 않으니 넘어가도 된다.

3. length 프로퍼티

함수 객체의 length프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

4. name 프로퍼티 

함수 객체의 name프로퍼티는 함수 이름을 나타낸다.
ES6이전에는 비표준이었지만 ES6부터 정식 표준이 되었다.
ES5와 ES6에서 동작을 다르게 하므로 주의해야 한다.
익명 함수 표현식의 경우 ES5에서는 name프로퍼티는 빈 문자열을 값으로 갖고,
ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

5. proto 접근자 프로퍼티

모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다.
[[Prototype]]내부 슬롯은 상속을 구현하는 프로토타입 객체를 가리킨다.

__proto__프로퍼티는 [[Prototype]]내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다. 내부슬롯에 직접 접근할 수 없어 간접적으로 접근하기 위한 프로퍼티이다.

6. proto 프로퍼티 
prototype프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티다.
일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype프로퍼티가 없다.

prototype프로퍼티는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.