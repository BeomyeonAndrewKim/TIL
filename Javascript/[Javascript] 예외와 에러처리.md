# 예외와 에러 처리

예외 처리(exception handling)는 에러를 컨트롤하는 메커니즘입니다. 

## 1. Error 객체

자바스크립트에는 내장된 ```Error``` 객체가 있습니다. ```Error``` 인스턴스를 만들면서 에러 메시지를 지정할 수 있습니다.

```javascript
const err = new Error('invalid email');
err //  Error: invalid email at eval(.....)
```

## 2. try/catch와 예외 처리

예외 처리는 ```try…catch``` 문을 사용합니다. 뭔가를 시도(try)하고, 예외가 있으면 그것을 캐치(catch)한다는 뜻이 잘 드러납니다. 예상치 못한 에러에 대처하려면  ```try...catch``` 문 코드 전체를 감쌀 수 있습니다.

```javascript
const email = null// ...

function validateEmail(email){
    return email.match(/@/)?
        email:
    	new Error(`invalid email:${email}`);
}

try {
    const validatedEmail = validateEmail(email);
    if(validatedEmail instanceof Error){
        console.error(`Error:${validatedEmail.message}`);
    } else {
        console.log(`Valid email: ${validatedEmail}`)
    }
} catch(err){
    console.error(`Error:${err.message}`);
}
```

에러를 캐치했으므로 프로그램은 멈추지 않습니다. 실행 흐름은 에러가 일어나는 즉시 ```catch``` 블록으로 이동합니다. 즉 ```try``` 블록에 ```if```문 부터 실행되지 않습니다. 

## 3. 에러 일으키기

자바스크립트가 에러를 일으키기만 기다릴 필요 없이 직접 에러를 일으켜서 예외 처리 작업을 할 수 있습니다.

예외 처리 기능이 있는 다른 언어와 달리, 자바스크립트는 에러를 일으킬 때 꼭 객체만이 아니라 숫자나 문자열 등 어떤 값이든 ```catch``` 절에 넘길 수 있습니다. 하지만 ```Error``` 인스턴스를 넘기는 것이 가장 편리합니다. 대부분의 ```catch``` 블록은 ```Error``` 인스턴스를 받을 것이라고 간주하고 만듭니다. 당신이 일으킨 에러를 어디서 캐치할지 항상 완벽하게 컨트롤 할 수 있는 것은 아닙니다. 

```throw```를 호출하면 현재 함수는 즉시 실행을 멈춥니다.

```javascript
function billPay(amount, payee, account){
    if(amount>account.balance)
        throw new Error('insufficient funds');
    account.transfer(payee,amount);
}
```

## 4. try..catch..finally

```try``` 블록의 코드가 ```HTTP``` 연결이나 파일 같은 일종의 '자원'을 처리할 때가 있습니다. 프로그램에서 이 자원을 계속 가지고 있을 수는 없으므로 에러가 있든 없든 어느 시점에서는 이 자원을 해제해야 합니다. ```try``` 블록에는 문을 원하는 만큼 쓸 수 있고, 그 중 어디서든 에러가 일어나서 자원을 해제할 기회가 아예 사라질 수도 있으므로 ```try``` 블록에서 자원을 해제하는 건 안전하지 않습니다. 에러가 일어나지 않으면 실행되지 않는 ```catch``` 블록 역시 안전하지 않습니다. 이런 상황에서 ```finally``` 블록이 필요합니다. ```fianlly``` 블록은 에러가 일어나든, 일어나지 않든 반드시 호출됩니다.

```javascript
try{
    console.log("this line is executed");
    throw new Error("whoops");
    console.log("this line is not...");
} catch(err){
    console.log("there was an error...");
} finally {
    console.log("...always executed");
    console.log("perform cleanup here");
}


//this line is executed
//there was an error...
//...always executed
//perform cleanup here
```

