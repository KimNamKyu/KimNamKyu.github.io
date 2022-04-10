---
title: '[JS]코어자바스크립트'
date: 2021-03-25 20:00:00
category: 'javascript'
draft: false
---

## 함수와 함수호출의 차이, 고차함수

> 함수의 호출은 `리턴값`으로 대체를 한다.

함수 선언식

```javascript
const add = (a, b) => a + b

function calculator(func, a, b) {
  return func(a, b)
}

add(3, 5) //8 함수 호출
calculator(add, 3, 5)
```

## 호출스택

> Stack = LIFO Queue = FIFO

```javascript
function c() {
  console.log('c')
}
function a() {
  console.log('a')
  function b() {
    console.log('b')
    c()
  }
  b()
}
a() // a b c
c() // c
```

호출스택 순서  
[anonoymous > a() > ~~log c~~ > b() > ~~log b~~ > c() > ~~log c~~ > ~~c()~~ > ~~b()~~ > ~~a()~~]

<img src="./../../assets/callstack.png" alt="callstack">

## 스코프 체인

> 스코프(범위)체인 `블록기준` 함수에서 어떤값에 접근이 가능한가?

## 호이스팅

변수선언하기 전에 접근하는 경우(TDZ) 자바스크립트는 호이스팅이 발생한다. TDZ를 피하는 코드를 작성하는 것을 권장한다.

## this

this는 함수가 호출될때 결정된다.

```js
const header = document.querySelector('header')
header.addEventListener('click', function () {
  console.log(this) //header
})

header.addEventListener('click', () => {
  console.log(this) // window
})
```

함수는 일급객체라 값으로 전달해줄수있어 실행할수있다.
addEventListener의 함수는 선언이지 호출이 아니다.
this는 호출할때 결정되는데 호출할때 명시적으로 눈에 보이지 않는다.
결론은 this를 알수없다.
하지만 this를 분석할수 없는게 맞지만 공식문서상에 addEventListener의 this는 태그로 결정된다.

화살표함수는 부모의 this를 따라가는데 call을 붙이지 못하고 실행되게 되어있다.

### bind, call, apply

bind
자체는 호출인데 a라는 함수의 this만 바꾸어 새로운 함수를 만든다.

```js
function a() {}
a.bind(window)
```

apply/call
apply는 호출까지 진행해준다.
매개변수들을 배열로 넣어준다.
call은 순서대로 넣어준다.

```js
a.apply(window) === a.bind(window)()
a.apply(this, [3, 5])
a.call(this, 3, 5)
```

### 블록스코프와 매개변수
