# 30. Date

## 1. Date 생성자 함수

### 1.1. new Date()

인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.

### 1.2. new Date(milliseconds)

숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

### 1.3. new Date(dateString)

날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date객체를 반환한다.

### 1.4. new Date(year, month[, day, hour, minute, second, millisecond])

연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

## 2. Date 메서드

### 2.1. Date.now

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

### 2.2. Date.parse

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지 밀리초를 숫자로 반환한다.

### 2.3. Date.UTC

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지 밀리초를 숫자로 반환한다.
new Date(year, month[, day, hour, minute, second, millisecond])와 같은 형식의 인수를 사용해야 한다.

### 2.4. Date.prototype.getFullYear

Date 객체의 연도를 나타내는 정수 반환한다.

### 2.5. Date.prototype.setFullYear

Date 객체의 연도를 나타내는 정수를 설정한다.

### 2.6. Date.prototype.getMonth

Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다.

### 2.7. Date.prototype.setMonth

Date 객체의 월을 나타내는 0 ~ 11의 정수를 설정한다.

### 2.8. Date.prototype.getDate

Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 반환한다.

### 2.9. Date.prototype.setDate

Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 설정한다.

### 2.10. Date.prototype.getDay

Date 객체의 요일(0 ~ 6)을 나타내는 정수를 반환한다.

### 2.11. Date.prototype.getHours

Date 객체의 시간(0 ~ 23)을 나타내는 정수를 반환한다.

### 2.12. Date.prototype.setHours

Date 객체의 시간(0 ~ 23)을 나타내는 정수를 설정한다.

### 2.13. Date.prototype.getMinutes

Date 객체의 분(0 ~ 59)을 나타내는 정수를 반환한다.

### 2.14. Date.prototype.setMinutes

Date 객체의 분(0 ~ 59)을 나타내는 정수를 설정한다.

### 2.15. Date.prototype.getSeconds

Date 객체의 초(0 ~ 59)을 나타내는 정수를 반환한다.

### 2.16. Date.prototype.setSeconds

Date 객체의 초(0 ~ 59)을 나타내는 정수를 설정한다.

### 2.17. Date.prototype.getMilliseconds

Date 객체의 밀리초초(0 ~ 999)을 나타내는 정수를 반환한다.

### 2.18. Date.prototype.setMilliseconds

Date 객체의 밀리초초(0 ~ 999)을 나타내는 정수를 설정한다.

### 2.19. Date.prototype.getTime

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

### 2.20. Date.prototype.setTime

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 설정한다.

### 2.21. Date.prototype.getTimezoneOffset

UTC와 Date 객체 지정된 로캘(locale) 시간과의 차이를 분 단위로 반환한다. KST는 UTC에 9시간을 더한 시간이다. 즉, UTC = KST - 9h다.

### 2.22. Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

### 2.23. Date.prototype.toTimeString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 시간을 표현한 문자열을 반환한다.

### 2.24. Date.prototype.toISOString

ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

### 2.25. Date.prototype.toLocaleString

인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

### 2.26. Date.prototype.toLocaleTimeString

인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.

## 3. Date를 활용한 시계 예제

```js
(function printNow() {
  const today = new Date();

  const dayNames = [
    "(일요일)",
    "(월요일)",
    "(화요일)",
    "(수요일)",
    "(목요일)",
    "(금요일)",
    "(토요일)",
  ];

  // getDay 메서드는 해당 요일(0 ~ 6)을 나타내는 정수를 반환한다.
  const day = dayNames[today.getDay()];

  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? "PM" : "AM";

  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당

  // 10미만인 분과 초를 2자리로 변경
  minute = minute < 10 ? "0" + minute : minute;
  second = second < 10 ? "0" + second : second;

  const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

  console.log(now);

  // 1초마다 printNow 함수를 재귀 호출한다. 41.2.1절 "setTimeout / clearTimeout" 참고
  setTimeout(printNow, 1000);
})();
```
