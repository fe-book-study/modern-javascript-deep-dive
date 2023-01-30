# 32. String

## 1. String 생성자 함수

표준 빌트인 객체인 String 객체는 생성자 함수 객체이다. 따라서 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.

String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}
```

String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String("Lee");
console.log(strObj); // String {0: "L", 1:"e", 2:"e", length: 3, [[PrimitiveValue]]: "Lee"}
```

String 생성하 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, [[StringData]] 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String(123);
console.log(strObj); // String {0: "1", 1:"2", 2:"3", length: 3, [[PrimitiveValue]]: "123"}
```

new 연산자를 사용하지 않고 String 생성자 함수를 호출하여 String 인스턴스가 아닌 문자열을 반환한다.

```js
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"
```

## 2. length 프로퍼티

length 프로퍼티는 문자열의 문자 개수를 반환한다.

## 3. String 메서드

문자열은 변경 불가능한 원시 값이기 때문에 **String 래퍼 객체도 읽기 전용 객체로 제공된다.**

### 3.1. String.prototype.indexOf

대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.

### 3.2. String.prototype.search

대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.

### 3.3 String.prototype.includes

ES6에서 도입된 includes 메서드는 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 true 또는 false로 반환한다.

### 3.4 String.prototype.startsWith

ES6에서 도입된 startsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 true 또는 false로 반환힌다.

### 3.5 String.prototype.endsWith

ES6에서 도입된 endsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true 또는 false로 반환한다.

### 3.6 String.prototype.charAt

대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.

### 3.7 String.prototype.substring

대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.

### 3.8 String.prototype.slice

substring 메서드와 동일하게 동작한다.

음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

### 3.9 String.prototype.toUpperCase

대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

### 3.10 String.prototype.toLowerCase

대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

### 3.11 String.prototype.trim

대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

### 3.12 String.prototype.repeat

ES6에서 도입된 repeat 메서드는 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다. 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고 음수이면 RangeError를 발생시킨다. 인수를 생략하면 기본값은 0이 설정된다.

### 3.13 String.prototype.replace

대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

### 3.13 String.prototype.split

대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 부리된 각 문자열로 이루어진 배열을 반환한다.
