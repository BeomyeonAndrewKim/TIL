### 자바스크립트에서 십진수 소수의 연산

#### 1. 자바스크립트 Number타입의 기본 연산

보통 자바스크립트에서 number타입은 연산자를 활용해 연산이 가능하다.

```javascript
1 + 2; // 더하기
3 - 4; // 빼기
5 * 6; // 곱하기
7 / 8; // 실수 나누기
14 % 3; // 나머지
2 ** 3; // 거듭제곱
```

#### 2. 십진수 소수 표기 방법

하지만 10진수 소수에 대한 연산을 했을 때는 아래 처럼 의아한(?) 결과가 나온다.

```javascript
0.1+0.2 // 0.30000000000000004
```

왜 그럴까.

먼저 컴퓨터는 **소수를 2진수를 이용해 저장**한다. 그렇기 때문에 위의 10진수 소수를 정확히 다룰 수 없다.

그리고 숫자는 [IEEE 754](https://ko.wikipedia.org/wiki/IEEE_754) 표준에 따라 두배 정확도의 [**부동 소수점 숫자(floating point numbers)**](https://ko.wikipedia.org/wiki/%EB%B6%80%EB%8F%99%EC%86%8C%EC%88%98%EC%A0%90)로 저장한다. 이것은 원주율([π](https://namu.wiki/w/%EC%9B%90%EC%A3%BC%EC%9C%A8)/3.1415926535897932384626433832795….)이나 10/3이 3.3333....이 나오는 것과 비슷하다고 볼 수 있다.

예를 들어보자.



먼저 정수 10을 입력하면 컴퓨터는 2진수로 바꾸면 아래와 같다.

![이진수](https://firebasestorage.googleapis.com/v0/b/fds-firebase-storage-eee6f.appspot.com/o/%E1%84%8B%E1%85%B5%E1%84%8C%E1%85%B5%E1%86%AB%E1%84%89%E1%85%AE%20%E1%84%91%E1%85%AD%E1%84%80%E1%85%B5%E1%84%87%E1%85%A5%E1%86%B8.001.jpeg.001.jpeg?alt=media&token=d271d9a8-8f94-436c-9943-1b4343925231)

컴퓨터는 2진수인 1010으로 10진수 10을 처리한다.

그렇다면 10진수 소수는 어떤식으로 표현될까?

0.1을 예를 들어보자

먼저 곱하기2를 해서 1보다 크면 1, 작으면 0으로해서 변경한다.

```.javascript 
0.1 x 2 = 0.2 이므로 1보다 작으니 0
0.2 x 2 = 0.4 이므로 1보다 작으니 0
0.4 x 2 = 0.8 이므로 1보다 작으니 0
0.8 x 2 = 1.6 1, 그리고 0.6은 다시 반복
0.6 x 2 = 1.2 1보다 크므로 1
0.2 x 2 = 0.4 이므로 0 

따라서 0.1를 이진수로 변경하면 0.000110011...
```

10진수 소수의 표기 방법은 [링크](http://daejin.blogspot.kr/2013/08/01-02.html)에서 자세하게 확인할 수 있다.

#### 3. 십진수 소수 연산 방법

이럴때 보통 곱셈과 나눗셈을 통해 십진수 소수의 연산을 가능하게 할 수 있다.

```javascript
(0.2 * 10 + 0.1 * 10) / 10;   // 0.3
```

아니면 결과값에 ```toFixed```메소드를 활용해 자릿수를 고정할 수 있다. 

```javascript
(0.1+0.2).toFixed(1) // 0.3
```







