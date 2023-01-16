# 28. Number

## 1. Number 생성자 함수

## 2. Number 프로퍼티

### 2.1. Number.EPSILON

ES6에서 도입된 Number.EPSILON은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.

Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

### 2.2. Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다.

Number.MAX_VALUE보다 큰 숫자는 Infinity다

### 2.3. Number.MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.

Number.MAX_VALUE보다 작은 숫자는 0이다

### 2.4. Number.MAX_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다

### 2.5. Number.MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다

### 2.6. Number.POSITIVE_INFINITY

양의 무한대를 나타내는 숫자값 Infinity와 같다

### 2.7. Number.NEGATIVE_INFINITY

음의 무한대를 나타내는 숫자값 -Infinity와 같다

### 2.8. Number.NaN

숫자가 아님을 나타내는 숫자값이다.

## 3. Number 메서드

### 3.1. Number.isFinite

ES6에서 도입된 Number.isFinite 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.

인수가 NaN이면 언제나 false를 반환한다.

### 3.2. Number.isInteger

ES6에서 도입된 Number.isInteger 정적 메서드는 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다. 검사하기전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

### 3.3. Number.isNaN

ES6에서 도입된 Number.isNaN 정적 메서드는 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

숫자가 아닌 인수가 주어졌을때 반환값은 언제나 false다

### 3.4. Number.isSafeInteger

ES6에서 도입된 Number.isSafeInteger 정적 메서드는 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

### 3.5. Number.prototype.toExponential

숫자를 지수 표기법으로 변환하여 문자열로 반환한다.

### 3.6. Number.prototype.toFixed

숫자를 반올림하여 문자열로 반환한다.

### 3.7. Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.

### 3.8. Number.prototype.toString

숫자를 문자열로 변환하여 반환한다. 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다.
