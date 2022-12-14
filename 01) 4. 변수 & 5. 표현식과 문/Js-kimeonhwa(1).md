Js 딥다이브



4장

#### 4-1 변수란 무엇인가? 왜 필요한가?

### JS 엔진이 `10+20` 이라는 식을 해석하는 과정

1) 10과 20이 메모리 상의 임의의 위치에 저장
2) cpu가 이 값을 읽어 연산 수행
3) 연산 결과인 30을 메모리에 저장

문제점 : 연산 결과인 30을 재사용하지 못하고, 해당 값에 접근하고 싶으면 메모리 주소를 통해 직접 접근해야 한다 

-> 재사용이 가능하도록 변수라는 메커니즘 도입



### 변수

- 도입 배경 : 메모리에 직접 접근하지 않고 값을 재사용하기 위해 만들어짐
- 의미 : 값의 위치를 가리키는 상징적인 이름

```js
var result = 10 + 20; // 할당
console.log(result); // 참조
```



> 변수란 값의 위치를 가리키는 상징적인 이름이고, 직접 메모리에 접근하지 않고 값을 재활용할 수 있기에 사용한다 



#### 4-2 식별자

- 메모리 주소에 붙인 이름
- 식별자가 기억하고 있는 메모리 주소를 통해 메모리 공간에 저장된 값에 접근할 수 있다
- 코드내에서 선언을 통해 js 엔진에게 식별자의 존재를 알린다 



#### 4-3 변수 선언

- 변수를 선언한다 = 메모리 공간을 확보하고, 변수이름과 확보된 메모리 공간의 주소를 연결
- var, let , const 키워드를 통해 
- 변수 선언 이후, 변수에 값을 할당하지 않아도 암묵적으로 undefined 라는 값이 할당되어 초기화 된다 



### JS 엔진 의 변수 선언 단계

1. 선언 단계 : 변수 이름을 등록
2. 초기화 단계 : 값을 저장하기 위한 메모리 공간을 확보, 암묵적으로 undefined라는 값을 할당
3. var 키워드를 사용한다면 1,2번이 동시에 수행된다 

> 변수를 선언할 때는 키워드를 통해서 선언





#### 4-4 변수 선언의 실행 시점과 변수 호이스팅

```js
console.log(score);
var score;
```

첫번째 줄에서 참조 에러가 나지 않는 이유

- 호이스팅에 의해 변수 선언문이 최상단으로 끌어올라오는 것처럼 동작하기 때문

> var 키워드를 사용하면 호이스팅에 의해 변수 선언문이 최상단으로 끌어올라오는 것처럼 동작



#### 4-5 값의 할당

```js
var score; //변수 선언 -> 런타임 이전 실행
score = 100; //값의 할당 -> 런타임 이후 실행

var score = 100; //단축 표현
```





#### 4-6 값의 재할당

재할당 : 현재 변수에 저장된 값을 버리고 새로운 값을 저장

const 키워드는 재할당이 금지 (상수 취급)

재할당 시 메모리 공간 안의 값을 변경하는 것이 아니라 새로운 메모리 공간을 확보 후, 그 메모리 공간에 값을 저장

### 가비지 콜렉터

- 어떠한 식별자와도 연결이 되어있지 않은 메모리상의 공간들을 정리

### 매니지드 언어

- js 는 메모리 관리를 언어 차원에서 담당



#### 4-7 식별자 네이밍 규칙

1. 특수문자를 제외한 문자, 숫자, _ , $ 를 포함할 수 있음
2. 숫자로 시작할 수 없음
3. 예약어 사용 불가

### 네이밍 컨벤션

- 변수, 함수 -> 카멜케이스(firtName)
- 생성자 함수, 클래스 -> 파스칼 케이스(FirstName)



---------------



5장 표현식과 문



#### 값

값(value) : 표현식(expression) 이 평가(Evalutate)되어 생성된 결과

> 모든 값은 메모리에 2진수(bit)의 나열로 저장된다
>
> 이때, 값은 데이터 타입을 가지며, 이에 따라서 값을 다르게 해석하게 된다
>
> 값 0100 0001을 Number로 해석하면 65 이지만 String 으로 해석하면 'A' 이다
>
> 변수는 하나의 값을 저장하기 위해 메모리에 확보한 공간 또는 이를 식별하기 위해 붙인 이름이다 
>
> 따라서, 변수에는 값이 할당된다



#### 리터럴

리터럴(literal) : 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법(notation)

> 자바스크립트는 런타임 시점에서 리터럴을 평가해 값을 생성한다
>
> 리터럴은 값을 생성하기 위해서 미리 약속된 표기법이다



### 표현식

표현식(expression) : 값(value)로 평가될수 있는 문(statement) 이다

즉, 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다

```js
var score = 100;
// 100은 리터럴이다. 자바스크립트 엔진에 의해 평가되어 값을 생성
// 리터럴은 그 자체로 표현식이다

var score = 50 + 50; 
// 50 + 50 은 리터럴과 연산자로 이뤄져 있다
// 50 + 50 도 평가되어 숫자 값 100을 생성하므로 표현식이다

score; // -> 100
// 변수 식별자를 참조하면 변수 값으로 평가된다
// 식별자 참조는 값을 생성하지는 않지만 값으로 평가되므로 표현식이다 
```



> 표현식은 리터럴, 식별자(변수, 함수 등의 이름), 연산자, 함수 호출 등의 조합으로 이뤄진다
>
> 모두 값으로 평가된다는 점은 동일하다
>
> 즉, 값으로 평가될 수 있는 문은 모두 표현식이다.



표현식과 값은 동치(equivalent) 다 

이것은 문법적으로 값이 위치할 수 있는 자리에는 표현식도 위치할 수 있다는 것을 의미한다





#### 문

문(statement) : 프로그램을 구성하는 기본단위자 최소 실행단위 

문의 집합으로 이뤄진 것이 프로그램이며, 문을 작성하고 순서에 맞게 나열하는 것이 프로그래밍이다 문은 여러 토큰(token)으로 구성된다

토큰(token): 문법적 의미를 가지며, 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소

예를 들어, 키워드, 식별자, 연산자, 리터럴, 세미콜론, 마침표 등의 기호는 문법적 의미를 가지며, 문법적으로 더 이상 나눌 수 없는 코드의 기본요소이므로 모두 토큰이다

> 문은 **명령문**이라고도 불리며, 문이 실행되면 명령이 실행되고, 무슨일인가가 일어나게 된다.
>
> 문은 선언문, 할당문, 조건문, 반복문 등으로 구분할 수 있다. 변수 선언문을 실행하면 변수가 선언되고, 할당문을 실행하면 값이 할당된다. 조건문을 실행하면 조건에 따라 실행할 코드블록이 결정되어 실행되고, 반복문을 실행하면 특정 코드블록을 반복 실행한다.



#### 표현식인 문과 표현식이 아닌 문

```js
// 변수 선언문은 값으로 평가될 수 없으모로 표현식이 아니다
var x; // 문이지만 표현식이 아님

x = 1 + 2; // 문이면서 표현식임


var foo = var x; // SyntaxError : Unexpected token var
// 변수 선언문은 표현식이 아니다. 따라서 변수 선언문을 값처럼 사용할 수 없다

var x; // 표현식이 아닌 문(변수 선언문)
x = 100; // 할당문은 표현식이면서 완전한 문이다

// 표현식인 문은 값처럼 사용할 수 있다.
var foo = x = 100;
console.log(foo) // 100;

// 할당문을 값처럼 변수에 할당했다. 할당문은 표현식이므로 값으로 평가된다. 즉 x = 100은 x 변수에 100을 할당한 값 100으로 평가된다. 따라서 foo 변수에는 100이 할당된다.
```

