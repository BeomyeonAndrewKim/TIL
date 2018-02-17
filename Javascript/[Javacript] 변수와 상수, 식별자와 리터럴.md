### 1. 변수와 상수

1) **변수(Variable**): 이름이 붙은 값, 그래서 그 이름을 활용해 해당 값을 다시 가져올 수 있게 해준다. 언제든 바뀔 수 있음.

- 선언

```javascript
var foo; 
let boo;

foo // undefiend
boo // undefiend
```

- 할당

```javascript
var foo= 1;
let boo= "String";

foo // 1
boo // "String"

foo=2;
boo= "Number";

foo // 2
boo // "Number"
```

2) **상수(Constant)**: ES6에서 새로 생기 문법, 한 번 할당한 값을 바꿀 수 없게 해준다. 

```javascript
const foo; //Uncaught SyntaxError: Missing initializer in const declaration 상수는 단순히 선언만 하면 SyntaxError에 걸립니다. 반드시 값을 할당해야합니다.

const foo = 5;
foo // 5

foo = 4 //Uncaught TypeError: Assignment to constant variable. 재할당 불가
```



### 2. 변수와 상수 중 어떤 것을 써야하나?

1) 되도록이면 변수보다는 상수를 사용해야 한다. 데이터의 값이 아무 때나 막 바뀌는 것보다는, 고정된 값이 이해하기 쉽다.

2) 루프 제어나, 시간이 지나면서 값이 바뀌는 경우에는 변수를 사용해야 한다. 



### 3. 식별자

변수와 상수, 함수 이름을 식별자(identifier)라 부른다. 

1) **식별자 규칙**

- 숫자, 알파벳, 달러기호($), 밑줄(_) 가 포함될 수 있다.
- 숫자로 시작되어서는 안된다.
- 예약어(Javascript에서 사용하는 단어 ex-let, const)는 식별자로 쓸 수 없다.
- 유니코드 문자도 쓸 수 있다. 

```javascript
const foo; // O
const _bar123; // O
const $; // O - jQuery가 이 식별자를 사용한다.
const 7seven; // X
const const; // X - 예약어는 식별자가 될 수 없다.
```

2) **식별자 표기법**

여러가지 표기법이 있지만 가장 대표적인 두 가지가 있다.

- 카멜 케이스(Camel Case)

```javascript
let currenTempC;
let anIdentifierName;
// 중간중간의 대문자가 낙타의 혹처럼 보인다고 해서 카멜케이스라 불리웁니다.
```

- 스네이크 케이스(Snake Case)

```javascript
let current_temt_c;
let an_identifier_name;
```



### 4. 리터럴

리터럴은 값의 표기법으로 프로그래밍 언어마다 값을 표현하는 여러가지 리터럴을 가지고 있다.

```javascript
1; // 정수 리터럴
2.5; // 부동 소수점 리터럴
'hello'; // 문자열 리터럴
true; // 진리값 리터럴
```



### 5. 원시 타입

자바스크립트의 원시 값은 아래와 같다. 이러한 원시 값들을 '**'불변성''**의 특징을 가지고 있다. 

- 숫자(1,2,3,4...)
- 문자열('a','b','c'…..)
- Boolean(true, false)
- null
- Undefiend
- Symbol

원시 타입은 ```typeof``` 연산자를 활용해 확인할 수 있다.

```javascript
typeof 1; // 'number'
typeof 'hello'; // 'string
```