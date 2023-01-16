# 31. RegExp

## 1. 정규 표현식이란?

자바스크립트는 펄의 정규 표현식 문법을 ES3부터 도입했다.

정규 표현식은 문자열 대상으로 패턴 매칭 기능을 제공한다.

```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = "010-1234-567팔";

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // -> false
```

## 2. 정규 표현식의 생성

```js
const target = "Is this all there is?";

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
regexp.test(target); // -> true
```

```js
const target = "Is this all there is?";

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // -> true
```

```js
const count = (str, char) => (str.match(new RegExp(char, "gi")) ?? []).length;

count("Is this all there is?", "is"); // -> 3
count("Is this all there is?", "xx"); // -> 0
```

## 3. RegExp 메서드

### 3.1. RegExp.prototype.exec

인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.

### RegExp.prototype.test

인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

### String.prototype.match

String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

## 4. 플래그

| 플래그 | 의미        | 설명                                                            |
| ------ | ----------- | --------------------------------------------------------------- |
| i      | Ignore case | 대소문자 구별하지 않고 패턴 검색한다.                           |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.                  |

## 5. 패턴

### 5.1. 문자열 검색

```js
const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target); // true

// target과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp); // ["is", input: "Is this all there is?", groups: undefined]
```

### 5.2. 임의의 문자열 검색

.은 임의의 문자 한 개를 의미한다. 문자의 내용은 무엇이든 상관없다.

### 5.3. 반복 검색

{m,n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.

{n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다. 즉, {n}은 {n,n}과 같다.

{n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

+는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. 즉, +는 {1,}과 같다.

?는 앞선 패턴이 최대 한번(0번) 이상 반복되는 문자열을 의미한다. 즉, ?sms {0,1}과 같다.

### 5.4. OR 검색

|은 or의 의미를 갖는다.

분해되지 안은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.

범위를 지정하려면 [] 내에 -를 사용한다.

\d는 숫자를 의미한다.

\w는 알파벳, 숫자, 언더스코어를 의미한다.

\W는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

### 5.5. NOT 검색

[ ... ] 내의 ^은 not의 의미를 갖는다.

### 5.6. 시작 위치로 검색

[ ... ] 밖의 ^은 문자열의 시작을 의미한다.

### 5.7. 마지막 위치로 검색

\$는 문자열의 마지막을 의미한다.

## 6. 자주 사용하는 정규표현식

### 6.1. 특정 단어로 시작하는지 검사

```js
const url = "https://example.com";

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true

/^(http|https):\/\//.test(url); // -> true
```

### 6.2. 특정 단어로 끝나는지 검사

```js
const fileName = "index.html";

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

### 6.3. 숫자로만 이루어진 문자열인지 검사

```js
const target = "12345";

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

### 6.4. 하나 이상의 공백으로 시작하는지 검사

```js
const target = " Hi!";

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
```

### 6.5. 아이디로 사용 가능한지 검사

```js
const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

### 6.6. 메일 주소 형식에 맞는지 검사

```js
const email = "ungmo2@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(
  email
); // -> true
```

### 6.7. 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = "010-1234-5678";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

### 6.8. 특수 문자 포함 여부 검사

```js
const target = "abc#123";

/[^A-Za-z0-9]/gi.test(target); // -> true

/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi.test(target); // -> true

// 특수 문자를 제거한다.
target.replace(/[^A-Za-z0-9]/gi, ""); // -> abc123
```
