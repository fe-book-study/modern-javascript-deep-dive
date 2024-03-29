# 27. 배열

## 1. 배열이란

배열은 여러 개의 값을 순차적으로 나열한 자료구조다

배열이 가지고 있는 값을 **요소**라고 부른다. 배열의 요소는 배열에서 자신의 위치를 나타내는 0 이상의 정수인 **인덱스**를 가진다. 인덱스는 배열의 요소에 접근할 때 사용한다. 배열은 요소의 개수, 즉 배열의 길이를 나타내는 **length 프로퍼티**를 갖는다.

배열은 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성할 수 있다.

| 구분            | 객체                      | 배열          |
| --------------- | ------------------------- | ------------- |
| 구조            | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조       | 프로퍼티 키               | 인덱스        |
| 값의 순서       | X                         | O             |
| length 프로퍼티 | X                         | O             |

## 2. 자바스크립트 배열은 배열이 아니다

배열의 요소가 연속적으로 이어져 있지 않은 배열을 **희소 배열**이라 한다. **자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.**

자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는 특수한 객체이다. 자바스크립트 배열의 요소는 사실 프로퍼티 값이다. 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 갑이라도 배열의 요소가 될 수 있다.

일반적인 배열과 자바스크립트 배열의 장단점

- 일반적인 배열은 인덱스로 요소에 빠르게 접근할 수 있다. 하지만 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않다.
- 자바스크립트 배열은 해시 테이블로 구현된 객체이므로, 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴수밖에 없는 구조적인 단점이 있다. 하지만 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.

## 3. length 프로퍼티와 희소 배열

length 프로퍼티 값은 요소의 개수, 즉 배열의 길이를 바탕으로 결정되지만 임의의 숫자 값을 명시적으로 할당할 수 있다. 현재 length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어든다.

주의할 것은 현재 length 프로퍼티 값보다 큰 숫자 값을 할당하는 경우다. 이때 length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.

length 프로퍼티 값을 할당하면 그 크기 만큼 배열의 길이가 줄어들 수 있다. 늘어날 수는 없다.
자바스크립트는 희소 배열을 문법적으로 허용한다.

**희소 배열은 length와 배열 요소의 개수가 일치하지 않는다.** **희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.**

**배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다.**

## 4. 배열 생성

### 4.1 배열 리터럴

```js
// 희소 배열
const arr = [1, , 3]; // [1, empty, 3]
```

### Array 생성자 함수

배열은 요소를 최대 2^32 - 1개 가질 수 있다. 전달된 인수가 범위를 벗어나면 RangeError가 발생한다.

```js
// 희소 배열
const arr = new Array(10); // [empty x 10]
const arr = new Array(); // []
const arr = new Array(1, 2, 3); // [1, 2, 3]
const arr = new Array({}); // [{}]
```

### Array.of

```js
Array.of(1); // [1]
Array.of(1, 2, 3); // [1, 2, 3]
Array.of("string"); // ['string']
```

### Array.from

ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.

```js
Array.from({ length: 2, 0: "a", 1: "b" }); // ['a', 'b']
Array.from("Hello"); // ['H', 'e', 'l', 'l', 'o']
Array.from({ length: 3 }); // [undefined, undefined, undefined]
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

## 5. 배열 요소의 참조

배열의 요소를 참조할 때는 대괄호([]) 표기법을 사용한다. 대괄호 안에는 인덱스가 와야 한다.

존재하지 않은 요소에 접근하면 undefined 반환된다.

## 6. 배열 요소의 추가와 갱신

존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다. 이 때 length 프로퍼티 값은 자동 갱신된다.

이미 존재한는 요소에 값을 재할당 하면 요소값이 갱신된다.

인덱스는 요소의 위치를 나타내므로 반드시 0 이상의 정수를 사용해야 한다. 만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성된다. 이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.

## 7. 배열 요소의 삭제

```js
const arr = [1, 2, 3];

delete arr[1]; // [1, empty, 3]
```

```js
const arr = [1, 2, 3];

// Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
arr.splice(1, 1); // [1, 3]
```

## 8. 배열 메서드

Array 생성자 함수는 정적 메서드를 제공하며, 배열 객체의 프로토타입인 Array.prototype은 프로토타입 메서드를 제공한다.

배열에는 원본 배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.

### 8.1. Array.isArray

전달된 인수가 배열이면 true, 배열이 아니면 false를 반환한다.

### 8.2. Array.prototype.indexOf

원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.

ES7에서 도입된 Array.prototype.includes 메서드를 사용하면 가독성이 더 좋다.

### Array.prototype.push

인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다.

ES6에서 도입된 스프레드 문법을 사용하면 함수 호출 없이 표현식으로 마지막에 요소를 추가할 수 있으며 부수 효과도 없다.

```js
const arr = [1, 2];
const newArr = [...arr, 3];
```

### Array.prototype.pop

원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다. 원본 배열이 빈 배열이면 undefined를 반환한다. 원본 배열을 직접 변경한다.

### Array.prototype.unshift

인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다. 원본 배열을 직접 변경한다.

### Array.prototype.shift

원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다. 원본 배열이 빈 배열이면 undefined를 반환한다. 원본 배열을 직접 변경한다.

### Array.prototype.concat

concat 메서드는 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다. 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소를 추가한다. 원본 배열은 변경되지 않는다.

push/unshift 메서드와 concat 메서드를 사용하는 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 것을 권장한다.

### Array.prototype.splice

원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다. 원본 배열을 직접 변경한다.

- start: 원본 배열의 요소를 제거하기 시작할 인덱스
- deleteCount: 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소의 개수
- items: 제거한 위치에 삽입할 요소들의 목록

### Array.prototype.slice

인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다. 원본 배열은 변경되지 않는다.

- start: 복사를 시작할 인덱스
- end: 복사를 종료할 인덱스

### Array.prototype.join

원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환한다. 구분자는 생략 가능하며 기본 구분자는 콤마(,)이다

### Array.prototype.reverse

원본 배열의 순서를 반대로 뒤집는다. 원본 배열이 변경된다. 반환값은 변경된 배열이다.

### Array.prototype.fill

ES6에서 도입된 fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다. 원본 배열이 변경된다.

### Array.prototype.includes

ES7에서 도입된 includes 메서드는 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환한다.

### Array.prototype.flat(depth)

ES10에서 도입된 flat 메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

## 9. 배열 고차 함수

고차함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다. 자바스크립트의 함수는 일급 객체이므로 함수를 값처럼 인수로 전달할 수 있으며 반환할 수도 있다. 고차 함수는 외부 상태의 변경이나 가변 데이터를 피하고 **불변성을 지향**하는 함수형 프로그래밍 기반을 두고 있다.

함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 **조건문과 반복문을 제거**하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임이다. **순수 함수를 통해 부수 효과를 최대한 억제**하여 오류를 피하고 프로그램의 안정성을 높이려는 노력의 일환이라고 할 수 있다.

### Array.prototype.sort

배열의 요소를 정렬한다. 원본 배열을 직접 변경하며 정렬된 배열을 반환한다. 기본적으로 오름차순으로 요소를 정렬한다.

sort 메서드의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따른다. 배열의 요소가 숫자 타입이라 할 지라도 배열의 요소를 일시적으로 문자열로 변환한 후 유니코드 코드 포인트의 순서를 기준으로 정렬한다.

따라서 숫자 요소를 정렬할 때는 sort 메서드에 **정렬 순서를 정의하는 비교 함수를 인수로 전달**해야 한다.

sort 메서드는 quicksort 알고리즘을 사용했었다. quicksort 알고리즘은 동일한 값의 요소가 중복되어 있을 때 초기 순서와 변경될 수 있는 불안정한 정렬 알고리즘으로 알려져 있다. ES10에서는 timsort 알고리즘을 사용하도록 바뀌었다.

### Array.prototype.forEach

forEach 메서드는 for 문을 대체할 수 잇는 고차 함수다. forEach 메서드는 자신의 내부에서 반복문을 실행한다.

forEach 메서드는 원본 배열을 변경하지 않는다.

forEach 메서드의 반환값은 언제나 undefined다

forEach 메서드는 for 문과 달리 break, continue 문을 사용할 수 없다.

forEach 메서드는 for 문에 비해 성능이 좋지는 않지만 가독성은 더 좋다.

### Array.prototype.map

map 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 **콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.** 이때 원본 배열은 변경되지 않는다.

### Array.prototype.filter

filter 메서드는 자신을 호출한 배열의 모든 요소 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 **콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.** 이때 원본 배열은 변경되지 않는다.

### Array.prototype.reduce(accumulator, currentValue, index, array)

reduce 메서드는 자신을 호출한 배열을 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 **하나의 결과값을 만들어 반환한다**. 이때 원본 배열은 변경되지 않는다.

### Array.prototype.some

자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다. 이때 some 메서드는 콜백함수의 반환값이 단 한 번이라도 참이면 true, 모두 거짓이면 false를 반환한다.

### Array.prototype.every

자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 홀출한다. 이때 every 메서드는 콜백 함수의 반환값이 모두 참이면 true, 단 한번이라도 거짓이면 false를 반환한다.

### Array.prototype.find

ES6에서 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다. 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 undefined를 반환한다.

### Array.prototype.findIndex

ES6에서 도입된 findIndex 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환한다. 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 -1을 반환한다.

### Array.prototype.flatMap

ES10에서 도입된 flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화한다. 즉, map 메서드와 flat 메서들르 순차적으로 실행하는 효과가 있다.
