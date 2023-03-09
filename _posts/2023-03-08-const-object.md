---
title: Array와 Object를 상수로 선언하는 것이 선호되는 이유
date: 2023-03-08 00:00:00 -0500
categories: ['study']
tags: ['Javascript', 'study']
---

### 이 글의 목적

코드 리뷰를 하다보면 `Array`와 `Object`를 `let`으로 선언한 것이 종종 보이는 경우가 있습니다.

`Array`에서 `push`, `pop`을 사용하거나 `Object`에 `key`, `value`를 추가할 때 저장된 값이 변경된다 생각하여 let으로 선언한 것으로 보여, 왜 `Array`와 `Object`를 `let`이 아닌 `const`를 사용하여 선언하는 것을 선호하는지에 대해 정리해보려 합니다.

### let과 const

먼저 `let`과 `const`에 대해서 이야기해보겠습니다.

`let` 과 `const` 는 ES6에서 추가된 `var`를 대체하는 변수 및 상수 선언 방식입니다.

`let` 은 변수입니다. 즉 변경이 가능합니다.

그래서 한 번 값을 지정하더라도 다른 값으로 변경할 수 있습니다.

만약 초기에 값을 할당하지 않았다면(`let a;`) `undefined`로 저장됩니다.

`const` 는 상수입니다. 즉 변경이 불가능합니다.

그래서 한 번 값을 지정하면 값을 변경할 수 없습니다.

초기에 값을 무조건 할당해줘야 합니다.

`var`와 `let`, `const` 의 차이점은 TDZ가 형성되는지 아닌지로 설명할 수 있으나 본문의 목적이 아니기 때문에 생략하겠습니다.

### 값의 선언

`let`과 `const`에 대해서 알아보았으니, 이번에는 값의 선언에 대해서 알아보겠습니다.

```jsx
let a = 10;
```

위와 같이 변수가 선언된다면 메모리에서는 무슨 일이 발생할까요?

변수에 저장되는 것은 10이라는 값이 아닌, 10이라는 값이 저장된 메모리의 주소값이 저장됩니다.

```jsx
console.log(a);
```

예를 들어 다음과 같은 코드를 실행했을 때 10이라는 값이 출력되는 프로세스를 살펴보겠습니다.

먼저 a라는 변수에 저장된 메모리의 주소값을 가져옵니다. 두번째로 데이터의 타입을 확인하여 메모리의 크기를 파악하고, 첫 번째 주소값에서부터 메모리의 크기만큼의 데이터를 읽어옵니다. 마지막으로 메모리에서 읽어들인 값을 해석 및 반환해줍니다.

다음과 같은 과정이 발생하기 때문에 `console.log(a);` 가 동작하면 `10`이라는 값이 반환됩니다.

즉, `let`은 메모리의 주소값이 변경될 수 있는 것이고 `const`는 메모리의 주소값이 변경될 수 없음을 의미합니다.

### Array, Object에서 const가 let보다 선호되는 이유

> 배열이나 객체에 추가할 때 상수를 재할당하거나 재선언하는 것이 아니라 이미 선언되고 할당된 상수가 가리키는 "목록"에 추가할 뿐입니다.

`Array`와 `Object`의 값을 가리키는 것이 아닌, 주소값을 가리키는 것이기 때문에 `Array`나 `Object`의 목록에 무언가 추가되더라도 주소값 자체가 변경되는 것이 아니기 때문에 `const`로 선언한 `Array`에 `push`, `pop`을 값 추가 및 제거, `Object`에 속성 추가 및 제거가 가능합니다.

`const`를 사용하더라도 `let`을 사용하더라도 추가 및 제거가 가능한 상태라면 재할당이 불가능한 `const`를 사용하는 것이 바람직하고 선호됩니다.

```jsx
const a = [];
a.push('a');
a = 'a'; // Uncaught TypeError: Assignment to constant variable.
```

위 코드를 보면 `let`을 사용한다면 ‘a’로 재할당 되어버리지만, `const`를 사용한다면 재할당을 막을 수 있습니다.

네이밍이 비슷한 경우가 존재할 수 있으므로 재할당을 막을 수 있는 `const`를 사용하는 것이 더 바람직하고 선호됩니다.

### 참고자료

[var, let, const 특징과 차이점](https://velog.io/@haleyjun/JavaScript-var-let-const-%EC%B0%A8%EC%9D%B4%EC%A0%90)

[배열을 const 로 선언했을 때, 왜 push와 pop이 가능할까 (state 배열)](https://morohaji.tistory.com/55)

[JavaScript's Memory Management Explained | Felix Gerschau](https://felixgerschau.com/javascript-memory-management/)

[자바스크립트의 메모리 관리 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management)

[[자바스크립트] 메모리 구조, 원시타입 변수 생성 원리, 가비지컬렉터](https://curryyou.tistory.com/275)

[javascript - object 객체 속성 추가, 삭제](https://m.blog.naver.com/designondo/221251431632)
