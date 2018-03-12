## 비동기적 Javascript

비동기적 Javascript는 여러개의 연산이 필요한 코드가 있을때 코드실행을 멈추지 않고 다음 코드를 먼저 실행하는 Javascript의 특징을 말합니다.

브라우저에서의 비동기 프로그래밍은 주로 통신, 계산과 같이 오래 걸리는 작업들을 브라우저에 위임할 때 이루어집니다.

비동기 프로그래밍 방식은 대개 프로그램의 성능과 응답성을 높이는 데에 도움을 줍니다. 하지만 코드가 실제로 실행되는 순서가 뒤죽박죽이 되므로, 코드의 가독성을 해치고 디버깅을 어렵게 만든다는 비판을 받아왔습니다.

이런 문제를 해결하기 위해 비동기 프로그래밍을 위한 여러 기법이 생겨났습니다. 그 중 주요 세가지를 살펴보겠습니다.

### 콜백함수

콜백함수란 함수의 인자로서 함수가 들어가는 것을 말합니다. 이를 활용한 가장 대표적인 비동기식 처리는 jQuery를 활용한 ajax통신입니다.

```javascript
function getData() {
	var tableData;
	$.get('url 주소/products/1', function (response) {
		callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
	});
}
getData(function (tableData) {
	console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

```$get``` 함수가 ajax통신을 하는 부분입니다. 이 함수는 두번째 인자로 함수를 받습니다. 이 함수는 첫번째 인자에 들어간 URL과의 통신을 통해 받아온 데이터를 다루는 함수입니다. 

```javascript
$.get('url', function (response) {
	parseValue(response, function (id) {
		auth(id, function (result) {
			display(result, function (text) {
				console.log(text);
			});
		});
	});
});
```

하지만 콜백함수로 비동기식 처리를 할 때는 단점이 있습니다. 위 방식처럼 데이터를 받아올 때 데이터를 활용해 여러가지 처리를 해줘야할 경우에는 콜백헬이라고 불리우는 콜백이 계속 콜백을 무는 형식으로 코딩을 진행하게된다는 점입니다. 이 점은 코드의 가독성을 매우 악화시킵니다.

출처: [CAPTAIN PANGYO BLOG](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/)

### Promise

```Promise```는 비동기식 프로그래밍을 위해 ES6에 새로 추가된 문법입니다. ```Promise```는 **'언젠가 끝나는 작업'의 결과값**을 담는 통과 같은 객체입니다.  

```Promise``` 객체가 만들어지는 시점에는 그 통 안에 무엇이 들어갈지 모를 수도 있습니다. 대신 `then` 메소드를 통해 콜백을 등록해서, 작업이 끝났을 때 결과값을 가지고 추가 작업을 할 수 있습니다.

```javascript
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('2초가 지났습니다.');
    resolve('hello');
  }, 2000);
});
```

위 코드 처럼 ```resolve```와 ```reject```를 인자로 받아 비동기 프로그래밍을 할 수 있습니다. Promise 생성자의 인스턴스에는 ```resolve```와 ```reject```값이 들어있는 객체가 들어가게 됩니다. 

그리고 그 인스턴스는 ```then``` 메소드를 활용해 결과값으로 추가 작업을 할 수 있습니다.

```Fetch API```에서는```fetch```함수를 통해 HTTP통신을 할 수 있는데 ```fetch```함수는 ```Promise```객체를 반환합니다. 이를 통해 비동기식 프로그래밍을 합니다.

Promise 비동기 프로그래밍은 객체를 반환하고 이를 통해 비동기식 처리를 하기 때문에 콜백을 중첩하지 않습니다. 이는 콜백함수를 통한 비동기식 프로그래밍에 비해 가독성 좋은 코드를 작성할 수 있도록 해줍니다. 

하지만 복잡한 데이터 처리를 할 때는```then```메소드를 연이어 사용해야하는 상황이 다시 발생하기 때문에 콜백함수에 비해서는 가독성이 좋지만 여전히 절대적인 가독성은 여전히 좋지 않습니다.

출처: [자바스크립트로 만나는 세상](https://helloworldjavascript.net/pages/285-async.html)

### async~await

이를 보완하기 위해 ES2017에 **비동기 함수(async function)** 문법이 도입되었습니다. 

```javascript
// 비동기 함수
async function func1() {
  // ...
}

// 비동기 화살표 함수
const func2 = async () => {
  // ...
}

// 비동기 메소드
class MyClass {
  async myMethod() {
    // ...
  }
}
```

위 코드처럼 함수 선언할 때 ```async```를 붙여주면 비동기 함수가 됩니다.

비동기 함수는 항상 ```Promise```객체를 반환합니다. 이는 ```then```메소드도 동일하게 동작한다는 것을 의미합니다.

비동기 함수의 중요한 특징은 ```await```키워드입니다. Promise의 ```then```메소드와 유사한 기능을 하는데 , ```await```키워드 뒤에 오는 ```Promise```가 결과 값을 가질때까지 비동기 함수의 실행을 중단시킵니다. 

```await```는 연산자이기도 하며, 결과값은 뒤에 오는 ```Promise```객체의 결과값이 됩니다. 

비동기 함수의 장점은 콜백함수 중첩이나 ```then```메소드를 연이어 쓸 필요없이 동기식 코드처럼 쓸 수 있다는 점입니다. 

하지만 ```Promise```를 사용하기 때문에 이를 잘 모르면 활용하기가 어렵습니다.