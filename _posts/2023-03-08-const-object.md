---
title: Array, Object를 상수로 선언하는 것을 권장하는 이유
date: 2023-03-08 00:00:00 -0500
categories: ['study']
tags: ['Javascript', 'study']
---

## 이 글의 목적

<p>
코드 리뷰를 하다 보면 `Array`와 `Object`를 `let`으로 선언한 것을 종종 본 적이 있었습니다.
<br />
본문에서는 왜 `Array`, `Object`를 선언할 때 왜 `let`으로 선언하는 것이 위험한지, 왜 `const`로 선언하는 것을 권장하는지 알아보도록 하겠습니다.
</p>

## let과 const

<p>
먼저 `let`과 `const`에 대해서 이야기해보겠습니다.
<br />
`let` 과 `const` 는 ES6에서 추가된 `var`를 대체하는 변수 및 상수 선언 방식입니다.
</p>

<p>
`let`은 변수입니다. 즉 변경이 가능합니다.
<br />
그래서 한 번 값을 지정하더라도 다른 값으로 변경할 수 있습니다.
<br />
만약 초기에 값을 할당하지 않았다면 메모리가 비어있는 것이 아니라 `undefined` 값이 할당되어 있습니다.
</p>

<p>
`const` 는 상수입니다. 즉 변경이 불가능합니다.
<br />
그래서 한 번 값을 지정하면 값을 변경할 수 없습니다.
<br />
초기에 값을 무조건 할당해줘야 합니다.
</p>

<p>
`var`와 `let`, `const` 의 차이점은 ?TDZ가 형성되는지 아닌지로 설명할 수 있으나 본문에서 알아보려는 목적이 아니기 때문에 생략하겠습니다.
</p>

## 값의 선언

<p>
`let`과 `const`에 대해서 알아보았으니, 이번에는 값의 선언에 대해서 알아보겠습니다.
</p>

```JSXjsx
let a = 10;
```

<p>
위와 같이 변수가 선언된다면 메모리에서는 무슨 일이 발생할까요?
<br />
변수에 저장되는 것은 10이라는 값이 아닌, 10이라는 값이 저장된 메모리의 주소값이 저장됩니다.
</p>

```JSXjsx
console. log(a)console.log(a);
```

<p>
예를 들어 다음과 같은 코드를 실행했을 때 10이라는 값이 출력되는 프로세스를 살펴보겠습니다.
<br />
먼저 a라는 변수에 저장된 메모리의 주솟값을주소값을 가져옵니다. 두 번째로 데이터의 타입을 확인하여 메모리의 크기를 파악하고, 첫 번째 주솟값에서부터주소값에서부터 메모리의 크기만큼의 데이터를 읽어옵니다. 마지막으로 메모리에서 읽어 들인 값을 해석 및 반환해줍니다.
<br />
다음과 같은 과정이 발생하기 때문에 `console. log(a)console.log(a);` 가 동작하면 `10`이라는 값이 반환됩니다.
<br />
즉, `let`은 메모리의 주솟값이 변경될 수 있고 `const`는 메모리의 주소값이 변경될 수 없음을 의미합니다.
</p>

## const가 let보다 권장되는 이유

> 배열이나 객체에 추가할 때 상수를 재할당하거나 재선언하는 것이 아니라 이미 선언되고 할당된 상수가 가리키는 "목록"에 추가할 뿐입니다.

<p>
`Array`와 `Object`의 값을 가리키는 것이 아닌, 주소값을 가리키는 것이기 때문에 `Array`나 `Object`의 목록에 무언가 추가되더라도 주소값 자체가 변경되는 것이 아니기 때문에 `const`로 선언한 `Array`에 `push`, `pop`을 값 추가 및 제거, `Object`에 속성 추가 및 제거가 가능합니다.
<br />
`const`와 `let` 중 어떤 것을 사용하더라도 추가 및 제거가 가능한 상태라면 혹시 모를 에러를 방지하기 위하여 재할당이 불가능한 `const`를 사용하는 것이 권장됩니다.
</p>

```jsx
const a = [];
a.push('a');
a = 'a'; // Uncaught TypeError: Assignment to constant variable.
```

## 참고자료

- [var, let, const 특징과 차이점](https://velog.io/@haleyjun/JavaScript-var-let-const-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- [배열을 const 로 선언했을 때, 왜 push와 pop이 가능할까 (state 배열)](https://morohaji.tistory.com/55)
- [JavaScript's Memory Management Explained | Felix Gerschau](https://felixgerschau.com/javascript-memory-management/)
- [자바스크립트의 메모리 관리 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management)
- [[자바스크립트] 메모리 구조, 원시타입 변수 생성 원리, 가비지컬렉터](https://curryyou.tistory.com/275)
- [javascript - object 객체 속성 추가, 삭제](https://m.blog.naver.com/designondo/221251431632)
- [JS : 선언자 let, const가 var보다 권장되는 이유](https://wnsdufdl.tistory.com/40)
- [let - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)
