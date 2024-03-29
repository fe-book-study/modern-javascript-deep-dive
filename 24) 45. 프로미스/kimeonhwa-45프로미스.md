자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다. 하지만 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처치가 곤란하며 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있기 때문에 프로미스가 등장하였다.

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

### 45.1.1 콜백 헬

비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료된다.

```js
let g = 0

setTimeout(() => {
  g = 100
}, 0)
console.log(g) // 0
```

위처럼 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

이처럼 콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리결과를 가지고 또 다시 비동기 함수를 호출하여 복잡해지는 현상을 **콜백 헬**이라 한다.

### 45.1.2 에러 처리의 한계

비동기 처리를 위한 콜백 패턴의 문제점 중에서 가장 심각한 것은 에러 처리가 곤란하다는 것이다.

```js
try {
  setTimeout(() => {
    throw new Error('error')
  }, 1000)
} catch (e) {
  // 에러를 캐치하지 못한다.
  console.log(error)
}
```



## 45.2 프로미스의 생성

Promise는 ECMAScript 사양에 정의된 표준 빌트인 객체다. Promise 생성자 함수는 비동기 처리를 수행할 콜백함수를 인수로 전달받는데 이 콜백함수는 resolve와 reject 함수를 인수로 전달받는다.

```js
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
  if(비동기 처리 성공){
    resolve('result')
  } else {
    reject('failure reason')
  }
})
```

비동기 처리가 성공하면 resolve 함수를 호출하고 비동기 처리가 실패하면 reject 함수를 호출한다.



### 비동기 작업이 가질 수 있는 3가지 상태

- Pending(대기 상태)
- Fulfilled(성공)
- Rejected(실패)

Pending 상태에서 Fulfilled로 해결이 되었을 때는 reslove가 된 것이고 Pending상태에서 Rejected상태로 거부 되었을 때는 reject 된 것이다.



### Promise

어떤 함수가 Promise를 반환한다는 것은 그 함수는 비동기 적으로 동작을 하고 반환한 Promise객체를 이용해서 비동기 처리의 결과 값을 then과 catch로 이용하겠다는 의미이다.





## 45.3 프로미스의 후속 처리 메서드

### 45.3.1 Promise.prototype.then

then 메서드는 두 개의 콜백 함수를 인수로 전달받는다.

- 첫번째 콜백 함수: 프로미스가 fulfilled 상태가 되면 호출되고 프로미스의 비동기 처리 결과를 인수로 전달받는다.
- 두번째 콜백 함수: 프로미스가 rejected 상태가 되면 호출되고 프로미스의 에러를 인수로 전달받는다.

then메서드는 언제나 프로미스를 반환한다.



### 45.3.2 Promise.prototype.catch

catch 메서드의 콜백함수는 프로미스가 rejected상태인 경우만 호출된다.



### 45.3.3 Promise.prototype.finally

finally 메서드의 콜백 함수는 프로미스의 성공 혹은 실패와 상관없이 무조건 한 번 호출된다.





## 45.4 프로미스의 에러 처리

- catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러 뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.
- catch 메서드를 사용하는 것이 또한 가독성이 좋고 명확하다.



## 45.4 프로미스 체이닝

then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다. 이를 **프로미스 체이닝**이라 한다.





## 45.6 프로미스의 정적 메서드

### 45.6.1 Promise.resolve / Promise.reject

두 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다. Promise.resolve 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성한다. Promise.reject 메서드는 인수로 전달받은 값을 reject하는 프로미스를 생성한다.



### 45.6.2 Promise.all

Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.

```js
Promise.all([비동기 처리 함수, 비동기 처리 함수])
  .then(console.log)
  .catch(console.error);
```

Promise.all 메서드는 프로미스는 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 그리고 전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환한다.



### 45.6.3 Promise.race

Promise.race는 Promise.all처럼 모든 프로미스가 fulfilled 상태가 되는 것을 기다리는 것이 아니라 **가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve**하는 새로운 프로미스를 반환한다.



### 45.6.5 Promise.allSettled

Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 그리고 전달받은 프로미스가 모두 settled상태(fulfilled || rejected)가 되면 처리 결과를 배열로 반환한다.





## 45.7 마이크로태스크 큐

프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장된다.
**마이크로태스크 큐는 태스크큐보다 우선순위가 높다.** 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한다. 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.





## 45.8 fetch

fetch는 XMLHttpRequest와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API다. 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.

**fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.**

```js
fetch(url)
  .then(res => {
  if(!res.ok) throw new Error(res.statusText)
  return res.json()
})
```