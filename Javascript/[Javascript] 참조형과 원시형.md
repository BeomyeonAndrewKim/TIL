## 참고형과 원시형

원시 값은 불변이고, 원시 값을 복사/전달할 때는 값 자체를 복사/전달. 따라서 '원본'의 값이 바뀌더라도 '사본'의 값이 따라서 바뀌지는 않음.

```javascript
let a=1 //원본
let b=a //사본. b는 1, a가 아님.
a=2 // 원본의 값을 바꿈.
console.log(b) //1. 사본의 값은 바뀌지 않음
```

또한 값 자체를 복사했으므로 변수와 값은 일치하지 않음.

```javascript
a===2 //true
```

값 자체를 전달하므로 함수 안에서 변수의 값이 바뀌어도 함수 외부에서는 바뀌지 않은 상태로 남음.

```javascript
function change(a){
    a=5
}
a=3
change(a)
console.log(a) //3
```

객체는 가변이고, 객체를 복사/전달할 때는 객체가 아니라 그 객체를 가리키고 있다는 사실(참조)을 복사/전달. 따라서 원본이 바뀌면 사본도 따라서 바뀜. 이런 특징을 강조하고 싶을 때 객체를 참조타입이라 부름.

```javascript
let o ={a:1}
let p=o // 이제 p는 o가 '가리키고 있는 것'을 가리킴.
o.a=2
console.log(p) ///{a:2}
```

다음 코드는 앞의 예제와 비슷해 보이지만 결과는 전혀 다름.

```javascript
let o={a:1}
let p=o // 이제 p는 o가 '가리키고 있는 것'을 가리킴.
p===o // true
o={a:2} // 이제 o는 다른 것을 가리킵니다. {a:1}을 수정한 것이 아님.
p===o // false
console.log(p)//{a:1}
```

객체를 가리키는 변수는 그 객체를 가리키고 있을 뿐, 객체 자체는 아니다. 따라서 변수와 객체는 결코 일치하지 않음

```javascript
let q={a:1};
q==={a:1} //false
```

참조를 전달하므로 함수 안에서 객체를 변경하면 함수 외부에서도 바뀜

```javascript
function change_o(o){
    o.a=999;
}

let o = {a:1};
change_o(o);
console.log(o) //{a:999}
```

Reference [Learning JavaScript](http://www.hanbit.co.kr/store/books/look.php?p_code=B2328850940)