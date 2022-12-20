## 12. 함수

### 12.1 함수란?

프로그래밍 언어의 함수는 일련의 과정을 문으로 구현하고 코드 블럭으로 감싸서 하나의 실행 단위로 정의한것이다.

```javascript
function add(x, y) {
      return x + y;
    }
    add(2, 5);
```

### 12.2 함수를 사용하는 이유

코드 재사용, 유지보수의 편의성 높임, 코드의 신뢰성 높임, 코드의 가독성 향상

### 12.3 함수 리터럴

자바스크립트의 함수는 객체 타입의 값임. 함수도 함수 리터럴로 생성 가능.
함수 리터럴은 함수 이름 생략 가능.

```javascript
    // 변수에 함수 리터럴을 할당
    var f = function add(x, y) { // f : 식별자, add : 함수 이름
      return x + y;
    };
    console.log(f(2,5)); // 식별자로 함수 호출!
```

### 12.4 함수 정의

1. 함수 선언문

```javascript
    // 함수 선언문
    function add(x, y) {
      return x + y;
    }
```

함수 리터럴과 형태가 동일하지만 함수 선언문은 함수 이름을 생략할 수 없음.

함수 선언문은 표현식이 아닌 문임, 변수에 할당 불가능.

2. 함수 표현식

```javascript
    // 함수 표현식
    var add = function (x, y) {
      return x + y;
    };

    console.log(add(2, 5)); // 7
```

3. 함수 생성 시점과 함수 호이스팅

```javascript
    // 함수 참조
    console.dir(add); // ƒ add(x, y)
    console.dir(sub); // undefined

    // 함수 호출
    console.log(add(2, 5)); // 7
    console.log(sub(2, 5)); // TypeError: sub is not a function

    // 함수 선언문
    function add(x, y) { // add 함수가 호이스팅 됨.
      return x + y;
    }

    // 함수 표현식
    var sub = function (x, y) { // var sub 까지만 호이스팅 됨.
      return x - y;
    };
```

함수 호이스팅 : 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작함.

함수 표현식으로 함수를 정의하면 함수 호이스팅이 아니라 변수 호이스팅이 발생함을 주의할 것!

4.  Function 생성자 함수

5. 화살표 함수

```javascript
    // 화살표 함수
    const add = (x, y) => {
        return x + y;
    }
    console.log(add(2, 5)); // 7
```

### 12.5 함수 호출

📍 매개변수의 최대 개수

충분히 많은 매개변수 지정 가능.
but 이상적인 함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 함.
매개변수 3개 이하 권장. (그 이상 필요하면 객체를 만들자. 객체를 인수로 전달.)

### 12.6 참조에 의한 전달과 외부 상태의 변경

원시값은 값 자체가 복사되어 전달되고 객체는 참조값이 복사되어 전달됨.

```javascript
    // 매개변수 primitive는 원시값을 전달받고, 매개변수 obj는 객체를 전달받는다.
    function changeVal(primitive, obj) {
      primitive += 100;
      obj.name = 'Kim';
    }

    // 외부 상태
    var num = 100;
    var person = { name: 'Lee' };

    console.log(num); // 100
    console.log(person); // {name: "Lee"}

    // 원시값은 값 자체가 복사되어 전달되고 객체는 참조값이 복사되어 전달된다.
    changeVal(num, person);

    // 원시값은 원본이 훼손되지 않는다.
    console.log(num); // 100

    // 객체는 원본이 훼손된다.
    console.log(person); // {name: "Kim"}
```

### 12.7 다양한 함수의 형태

1. 즉시 실행 함수

```javascript
    // 익명 즉시 실행 함수
    (function () {
      var a = 3;
      var b = 5;
      return a * b;
    }());
```
2. 재귀 함수 : 재귀 호출(자기 자신을 호출하는 것)을 수행하는 함수
3. 중첩 함수(내부 함수) : (외부)함수 내부에 정의된 함수. 외부 함수 내부에서만 호출 가능.
4. 콜백 함수 : 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수.
5. 고차 함수 : 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수.

콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수 전달 할 수 있음.
6. 순수 함수 : 외부 상태에 의존하지도 않고 변경하지도 않는, 즉 부수 효과가 없는 함수.
7. 비순수 함수 : 외부 상태에 의존하거나 외부 상태를 변경하는, 즉 부수 효과가 있는 함수.