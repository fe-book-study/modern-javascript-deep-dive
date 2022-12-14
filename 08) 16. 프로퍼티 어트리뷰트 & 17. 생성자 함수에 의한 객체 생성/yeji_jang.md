# 16장 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

내부 슬롯(internal slot), 내부 메서드(internal method)

- 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드

- 이중 대괄호로 감싼 이름

- ECMAScript 사양에 정의된 대로 구현되어 자바스크립트 엔진에서 실제로 동작

- 개발자가 직접 접근하도록 외부로 공개된 객체의 프로퍼티는 아님

- 종종 간접적으로 접근할 수 있는 수단을 제공함

  - `[[Prototype]]` 내부 슬롯 : 원칙적으로 직접 접근할 수 없지만, `__proto__` 를 통해 간접적으로 접근할 수 있음

    ``` js
    const o = {}
    
    o.[[Prototype]] // Error : Unexprected totken '['
    
    o.__Prototype__ // Object.prototype 
    ```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생설할 때 **프로퍼티의 상태**를 나타내는 `프로퍼티 어트리뷰트`를 기본값으로 **자동 정의**함

**프로퍼티의 상태**

- 값(value)
- 값의 갱신 가능 여부(writable)

- 열거 가능 여부(enumerable)
- 재정의 가능 여부(configurable)

**프로퍼티 어트리뷰트** 

- 자바스크립트 엔진이 관리하는 내부 상태 값(meta-property)인 `내부 슬롯 ([[Value]], [[Writable]], [[Enumerable]], [[Configurable]])` 이다.

- 내부 슬롯이기 때문에 직접 접근할 수 없지만, Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인 가능

- `Object.getOwnPropertyDescriptor(객체의 참조, 프로퍼티의 키)`

  - 프로퍼티 어트리뷰트 정보를 제공하는 `프로퍼티 디스크립터 객체`를 반환

    ```js
    const person = {
      name: 'Lee'
    }
    
    // name 프로퍼티에 해당하는, 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환
    console.log(Object.getOwnPropertyDescriptor(person, 'name'));
    // {value: "Lee", writble: true, enumerable: true, configurable: true}
    ```

- `Object.getOwnPropertyDescriptors(객체의 참조)`

  - 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공

    ```js
    const person = {
      name: 'Lee'
    }
    
    person.age = 20;
    
    // name 프로퍼티에 해당하는, 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환
    console.log(Object.getOwnPropertyDescriptor(person, 'name'));
    /* 
    {
    	name: {value: "Lee", writble: true, enumerable: true, configurable: true},
    	age: {value: 20, writble: true, enumerable: true, configurable: true}	
    }
    ```

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

**프로퍼티 종료**

- 데이터 프로퍼티
  - 키와 값으로 구성된 일반적인 프로퍼티
- 접근자 프로퍼티
  - 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수(accessor function)로 구성된 프로퍼티

### 16.3.1 데이터 프로퍼티

데이터 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 가짐

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                         |
| ------------------- | ----------------------------------- | ------------------------------------------------------------ |
| [[Value]]           | value                               | - 프로퍼티에 접근하면 반환되는 값 <br />- 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당<br />- 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장 |
| [[Writable]]        | writable                            | - 프로퍼티의 값 변경 가는 여부를 나타내는 불리언 값<br />- false인 경우 해당 프로퍼티는 읽기 전용이다. |
| [[Enumerable]]      | enumerable                          | - 프로퍼티의 열거 가능 여부를 나타내는 불리언 값<br />- false인 경우 for ... in 문이나, Object.keys 메서드 등으로 열거할 수 없음 |
| [[Configurable]]    | configurable                        | - 프로퍼티의 재정의 가능 여부를 나타내는 불리언 값<br />- false인 경우 프로퍼티 삭제, 프로퍼티 어트리뷰트 갑스이 변경이 금지됨<br />- 단, [[Writable]]이 true인 경우, [[value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용 됨 |

### 16.3.2 접근자 프로퍼티

접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 `데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수`로 구성된 프로퍼티

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                         |
| ------------------- | ----------------------------------- | ------------------------------------------------------------ |
| [[Get]]             | get                                 | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수<br />- 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수가 호출 되고 그 결과가 프로퍼티 값으로 반환됨 |
| [[Set]]             | set                                 | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수<br />- 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장됨 |
| [[Enumerable]]      | enumerable                          | -  데이터 프로퍼티의 [[Enumerable]]과 같다.                  |
| [[Configurable]]    | configurable                        | -  데이터 프로퍼티의 [[Configurable]]과 같다.                |

접근자 함수는 getter/setter 함수라고도 부르고, 둘 중 하나만 정의할 수도 있다.

```js
const person = {
  firstName: 'Ungmo',
  lastName: 'Lee',
  
  //fullName은 접근자 함수로 구성된 접근자 프로퍼티
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  }
  
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
};

console.log(person.fullName);  // Ungmo Lee

person.fullName = 'Heegun Lee';

console.log(person.fullName);  // Heegun Lee

let descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: f, set: f, enumerable: true, configurable: true}
```

- fullName에 접근하면, getter/setter 가 호출됨
- 접근자 프로퍼티는 [[Value]]를 가지지 않으며, 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여함
- fullName으로 프로퍼티 값에 접근하는 과정
  - 프로퍼티키가 유효한지 확인 (문자열 또는 심벌)
  - 프로토타입 체인에서 프로퍼티를 검색 (person 객체에 fullName 프로퍼티가 존재)
  - 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인 (fullName은 접근자 프로퍼티)
  - 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수를 호출하여 그 결과를 반환

> 프로토타입 prototype
>
> - 프로토타입은 어떤 객체의 상위 객체의 역할을 하는 객체
>
> - 프로토탕비은 하위 객체에게 자신의 프로퍼티와 메서드를 상속함
> - 프로토타입 객체의 프로퍼티나 메서드를 상속받은 하위 객체는 자신의 프로퍼티 또는 메서드인 것 처럼 자유롭게 사용할 수 있음
> - 프로토타입 체인은 포로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조를 의미
> - 객체의 프로퍼티나 메서드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메서드가 없다면 프로토타입 체인을 따라 프로토타입의 프로퍼티나 메서드를 차례대로 검색함

접근자 프로퍼티와 데이터 프로퍼티를 구분하는 방법

```js
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'); // 일반 객체의 __proto__는 접근자 프로퍼티
// {get: g, set: f, enumerable: false, configurable: true}

Object.getOwnPropertyDescriptor(function() {}, 'prototype'); // 함수 객체의 prototype은 데이터 프로퍼티
// {value: {...}, writable: true, enumerable: false, configurable: false}
```

## 16.4 프로퍼티 정의

프로퍼티 정의

- 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것
- `Object.defineProperty(객체의 참조, 데이터 프로퍼티의 키, 프로퍼티 디스크립터 객체)` 메서드를 사용해서 프로퍼티의 어트리뷰트를 정의할 수 있음

```js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true
});

Object.defineProperty(person, 'lastName', {
  value: 'Lee'
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);
// firstName {value: "Ungmo", writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// [[Enumerable]]의 값이 false인 경우
// 해당 프로퍼티는 for...in 문이나 Object.keys 등으로 열거할 수 없다.
// lastName 프로퍼티는 [[Enumerable]]의 값이 false이므로 열거되지 않는다.
console.log(Object.keys(person)); // ["firstName"]

// [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없다.
// lastName 프로퍼티는 [[Writable]]의 값이 false이므로 값을 변경할 수 없다.
// 이때 값을 변경하면 에러는 발생하지 않고 무시된다.
person.lastName = 'Kim';

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 삭제할 수 없다.
// lastName 프로퍼티는 [[Configurable]]의 값이 false이므로 삭제할 수 없다.
// 이때 프로퍼티를 삭제하면 에러는 발생하지 않고 무시된다.
delete person.lastName;

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 재정의할 수 없다.
// Object.defineProperty(person, 'lastName', { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName

descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
  // getter 함수
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set(name) {
    [this.firstName, this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true
});

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);
// fullName {get: ƒ, set: ƒ, enumerable: true, configurable: true}

person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

프로퍼티 디스크립터 객체에서 어트리뷰트를 생략하면 아래와 같이 기본 값이 적용됨

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| ----------------------------------- | ---------------------------- | -------------------- |
| value                               | [[Value]]                    | undefined            |
| get                                 | [[Get]]                      | undefined            |
| set                                 | [[Set]]                      | undefined            |
| writable                            | [[Writable]]                 | false                |
| enumerable                          | [[Enumerable]]               | false                |
| configurable                        | [[Configurable]]             | false                |

`defineProperties(객체의 참조, 여러개의 프로퍼티 객체)`

```js
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티 정의
  firstName: {
    value: 'Ungmo',
    writable: true,
    enumerable: true,
    configurable: true
  },
  lastName: {
    value: 'Lee',
    writable: true,
    enumerable: true,
    configurable: true
  },
  // 접근자 프로퍼티 정의
  fullName: {
    // getter 함수
    get() {
      return `${this.firstName} ${this.lastName}`;
    },
    // setter 함수
    set(name) {
      [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true
  }
});

person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

## 16.5 객체 변경 방지

객체는 변경가능한 값이므로, 재할당 없이 직접 변경할 수 있음

즉, 프로퍼티를 추가/삭제/갱신이 가능하며, Object.defineProperty 메서드를 사용하여 프로퍼티 어트리뷰트를 재정의 할 수도 있음

자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공함

| 구분           | 메서드                   | 추가 | 삭제 | 읽기 | 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| -------------- | ------------------------ | ---- | ---- | ---- | ---- | -------------------------- |
| 객체 확장 금지 | Obejct.preventExtensions | X    | O    | O    | O    | O                          |
| 객체 밀봉      | Object.seal              | X    | X    | O    | O    | X                          |
| 객체 동결      | Object.freeze            | X    | X    | O    | X    | X                          |

### 16.5.1 객체 확장 금지

`Obejct.preventExtensions`

- 객체의 확장(프로퍼티의 추가)를 금지
  - 프로퍼티 동적 추가, Object.defineProperty 두가지 방법 모두 금지
- `Object.isExtensible`로 확장가능한 객체인지 확인이 가능함

```js
const person = { name: 'Lee' };

// person 객체는 확장이 금지된 객체가 아니다.
console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지하여 프로퍼티 추가를 금지한다.
Object.preventExtensions(person);

// person 객체는 확장이 금지된 객체다.
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 추가는 금지되지만 삭제는 가능하다.
delete person.name;
console.log(person); // {}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지된다.
Object.defineProperty(person, 'age', { value: 20 });
// TypeError: Cannot define property age, object is not extensible
```

### 16.5.2 객체 밀봉

`Object.seal`

- 객체의 프로퍼티 추가/삭제/프로퍼티 어트리뷰트 재정의를 금지
- 즉, 객체의 프로퍼티 읽기/쓰기만 가능
- `Object.isSealed` 메서드로 확인이 가능함

```js
const person = { name: 'Lee' };// person 객체는 밀봉(seal)된 객체가 아니다.console.log(Object.isSealed(person)); // false// person 객체를 밀봉(seal)하여 프로퍼티 추가, 삭제, 재정의를 금지한다.Object.seal(person);// person 객체는 밀봉(seal)된 객체다.console.log(Object.isSealed(person)); // true// 밀봉(seal)된 객체는 configurable이 false다.console.log(Object.getOwnPropertyDescriptors(person));/*{  name: {value: "Lee", writable: true, enumerable: true, configurable: false},}*/// 프로퍼티 추가가 금지된다.person.age = 20; // 무시. strict mode에서는 에러console.log(person); // {name: "Lee"}// 프로퍼티 삭제가 금지된다.delete person.name; // 무시. strict mode에서는 에러console.log(person); // {name: "Lee"}// 프로퍼티 값 갱신은 가능하다.person.name = 'Kim';console.log(person); // {name: "Kim"}// 프로퍼티 어트리뷰트 재정의가 금지된다.Object.defineProperty(person, 'name', { configurable: true });// TypeError: Cannot redefine property: name
```

### 16.5.3 객체 동결

`Object.freeze`

- 객체를 동결한다.
- 객체를 읽는 것만 가능하게 만든다.
- `Object.isFrozen`으로 확인이 가능

```js
const person = { name: 'Lee' };

// person 객체는 동결(freeze)된 객체가 아니다.
console.log(Object.isFrozen(person)); // false

// person 객체를 동결(freeze)하여 프로퍼티 추가, 삭제, 재정의, 쓰기를 금지한다.
Object.freeze(person);

// person 객체는 동결(freeze)된 객체다.
console.log(Object.isFrozen(person)); // true

// 동결(freeze)된 객체는 writable과 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: false, enumerable: true, configurable: false},
}
*/

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신이 금지된다.
person.name = 'Kim'; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, 'name', { configurable: true });
// TypeError: Cannot redefine property: name
```

### 16.5.4 불변 객체

지금까지 살펴본 방지 메서드들은 얕은 변경만을 방지한다.

즉, 직속 프로퍼티만 변경이 방지되고, 중첩된 객체까지는 영향을 주지 못한다.

```js
const person = {
  name: 'Lee',
  address: { city: 'Seoul' }
};

// 얕은 객체 동결
Object.freeze(person);

// 직속 프로퍼티만 동결한다.
console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결하지 못한다.
console.log(Object.isFrozen(person.address)); // false

person.address.city = 'Busan';
console.log(person); // {name: "Lee", address: {city: "Busan"}}
```

객체의 중첩 객체까지 동결하려면, 객체를 값으로 가지는 모든 프로퍼티에 대해 Object.freeze 메서드를 재귀적으로 호출하여 적용하면 된다.

```js
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다.
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    /*
      모든 프로퍼티를 순회하며 재귀적으로 동결한다.
      Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
      ("19.15.2. Object.keys/values/entries 메서드" 참고)
      forEach 메서드는 배열을 순회하며 배열의 각 요소에 대하여 콜백 함수를 실행한다.
      ("27.9.2. Array.prototype.forEach" 참고)
    */
    Object.keys(target).forEach(key => deepFreeze(target[key]));
  }
  return target;
}

const person = {
  name: 'Lee',
  address: { city: 'Seoul' }
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결한다.
console.log(Object.isFrozen(person.address)); // true

person.address.city = 'Busan';
console.log(person); // {name: "Lee", address: {city: "Seoul"}}
```

# 17장 생성자 함수에 의한 객체 생성

### 17.1 Object 생성자 함수

---

- 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다.

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다.

  ```javascript
  // 빈 객체의 생성
  const person = new Object();
  ```

- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array등의 빌트인 생성자 함수를 제공한다.



<br>



### 17.2 생성자 함수

#### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

---

- 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하지만 단 하나의 객체만 생성한다.
  - 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

<br>

#### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

---

- 생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

  ```javascript
  // 생성자 함수
  function Circle(radius) {
      // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
      this.raduis = radius;
      this.getDiameter = function () {
          return 2 * this.radius;
      };
  }
  
  // 인스턴스의 생성
  const circle1 = new Circle(5);
  const circle2 = new Circle(10); 
  ```

  >  `this`는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. this가 가리키는 값은 함수 호출 방식에 따라 동적으로 결정된다.
- 자바와 같은 클래스 기반 객체지향언어의 생성자와는 다르게 그 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.` 만약, new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작한다.

<br>

#### 17.2.3 생성자 함수의 인스턴스 생성 과정

---

- 먼저, 생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 `인스턴스`를 생성하는 것과 `생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)`하는 것이다.
- new 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 `아래와 같은 과정`을 거쳐 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한 뒤 암묵적으로 인스턴스를 반환한다.

1. 인스턴스 생성과 this 바인딩 (런타임 이전에 실행)
   - 암묵적으로 빈 객체(인스턴스)가 생성된다.
   - 인스턴스는 `this`에 바인딩된다. -> 생성자 함수 내부의 `this`가 생성자 함수가 생성할 인스턴스를 가리키는 이유
2. 인스턴스 초기화
   - 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다.
     - 즉, 인스턴스에 프로퍼티나 메서드 추가, 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.
3. 인스턴스 반환
   - 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.
   - 만약, `this`가 아닌 다른 객체를 명시적으로 반환하면 `this`가 아니라 return 문에 명시한 객체가 반환된다.
   - 하지만, 명시적으로 원시 값을 반환하면 원시 값은 무시되고 `this`가 반환된다.

<br>

#### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

---

- 함수는 객체이지만 일반 객체와는 다르다 -> 일반 객체는 호출할 수 없지만 함수는 호출 가능하다.
- 따라서, 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]]등의 `내부 슬롯`과 [[Call]]. [[Construct]] 같은 `내부 메서드`를 추가로 가지고 있다.
- 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 `[[Call]]`이 호출된다.
- new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 `[[Construct]]`가 호출된다.
- 모든 함수 객체는는 내부 메서드 `[[Call]]`를 가지고 있지만 모든 함수 객체가 `[[Construct]]`를 갖는 것은 아니다. (모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니라는 뜻)

<br>

#### 17.2.5 constructor와 non-constructor의 구분

---

- constructor(생성자 함수로서 호출 가능한 함수) - 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)
- non-constructor(생성자 함수로서 호출할 수 없는 함수) - 메서드(ES6 메서드 축약 표현), 화살표 함수

<br>

#### 17.2.6 new 연산자

---

- new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.

```javascript
// 생성자 함수
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
}
```

- 위의 Circle 함수를 new 연산자와 함께 생성자 함수로서 호출하면 함수 내부의 `this`는 Circle 생성자 함수가 생성할 인스턴스를 가리킨다. 하지만 new 연산자없이 일반적인 함수로서 호출하면 `this`는 전역 객체 window를 가리킨다.



<br>



#### 17.2.7 new.target

----

- 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 `new.target`을 지원한다.
- `new.target`은 `this`와 유사해게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.
- 함수 내부에서 `new.target`을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.
  - new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 undefined다.
