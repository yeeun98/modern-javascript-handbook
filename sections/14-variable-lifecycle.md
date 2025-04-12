# 변수의 생명 주기와 전역 변수 관리

자바스크립트에서 **변수의 생명 주기**란 변수가 **언제 생성되고, 언제 소멸되는지**를 의미합니다.  
또한, **전역 변수는 편리하지만 관리하지 않으면 여러 가지 문제를 발생시킬 수 있습니다.**  
이 문서에서는 **변수의 생명 주기, 전역 변수의 문제점, 그리고 전역 변수를 피하는 방법**을 정리합니다.

---

## 📌 변수의 생명 주기
### 1️⃣ 지역 변수
> 함수 내부에서 선언된 **지역 변수**는 함수 실행 시 생성되고, 함수 종료 후 소멸됩니다.

```javascript
function foo() {
  var x = 'local';
  console.log(x); // 'local'
}

foo();
console.log(x); // ReferenceError (x는 지역 변수)
```

### 2️⃣ 전역 변수
> var로 선언된 전역 변수는 전역 객체의 프로퍼티로 등록되며, 프로그램이 종료될 때까지 유지됩니다.<br/>
> → 따라서 메모리를 계속 점유하며, 불필요한 전역 변수는 지양해야 함.
```javascript
var y = 'global';
console.log(window.y); // 'global'
```

---

## ⚠️ 전역 변수의 문제점

~~~
전역 변수 사용은 다음과 같은 부작용을 초래할 수 있음.
~~~

1️⃣ 암묵적 결합<br/>
	•	모든 코드가 전역 변수를 공유하므로 예상치 못한 변경 발생 가능성<br/>

2️⃣ 긴 생명 주기<br/>
	•	전역 변수는 프로그램이 종료될 때까지 유지되므로 메모리 낭비 가능성<br/>

3️⃣ 스코프 체인의 종점에 위치<br/>
	•	변수 검색 시 가장 마지막에 찾게 되므로 성능 저하<br/>

4️⃣ 네임스페이스 오염<br/>
	•	여러 파일에서 같은 이름의 전역 변수를 사용하면 충돌 위험 증가 ⚡<br/>

---

## ✅ 전역 변수 사용을 억제하는 방법

### 1️⃣ 즉시 실행 함수 (IIFE)

~~~
함수를 즉시 실행하여 변수의 범위를 지역화
~~~

```javascript
(function() {
  var localVar = "IIFE 내부 변수";
  console.log(localVar);
})(); 
console.log(localVar); // ReferenceError
```

### 2️⃣ 네임스페이스 객체 사용

~~~
전역 변수를 하나의 객체로 묶어 사용
~~~

```javascript
var MYAPP = {}; // 네임스페이스 객체 생성
MYAPP.name = "Lee";
console.log(MYAPP.name); // "Lee"
```

### 3️⃣ 모듈 패턴 (Module Pattern)

~~~
클래스를 흉내 내어 전역 변수 사용을 줄이는 패턴
~~~

```javascript
var Counter = (function(){
  var num = 0; // private 변수
  
  return {
    increase() { return ++num; },
    decrease() { return --num; }
  };
})();

console.log(Counter.increase()); // 1
console.log(Counter.decrease()); // 0
```

### 4️⃣ ES6 모듈 (Module)

~~~
모듈화하여 전역 변수 사용 최소화
~~~

```javascript
// lib.mjs
export const name = "Lee";

// app.mjs
import { name } from './lib.mjs';
console.log(name); // "Lee"
```

---

## 🎯 정리

✔ 지역 변수는 함수 실행 시 생성되고, 함수 종료 시 소멸됨.<br/>
✔ 전역 변수는 프로그램이 종료될 때까지 유지되므로 남발하면 메모리 낭비 및 성능 저하 발생.<br/>
✔ 전역 변수의 문제를 해결하기 위해<br/>
  - 즉시 실행 함수(IIFE)<br/>
  - 네임스페이스 객체<br/>
  - 모듈 패턴<br/>
  - ES6 모듈 등을 활용해야 함.<br/>
<br/>
**전역 변수는 최소화하고, 지역 변수를 적극 활용하는 것이 좋은 코드 스타일!**
