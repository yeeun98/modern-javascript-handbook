# 스코프란?
> **식별자가 유효한 범위**  
> 변수를 어디에서 선언하고 사용할 수 있는지를 결정하는 규칙.

### 📌 예제
```javascript
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x); // 'local'
}

foo();
console.log(x); // 'global'
```
- 함수 내부에서 선언한 변수는 함수 안에서만 사용 가능(지역 변수).
- 함수 바깥에서 선언한 변수는 어디서든 사용 가능(전역 변수).

💡 **스코프는 네임스페이스 역할을 하므로, 서로 다른 스코프에서는 같은 이름의 변수를 사용할 수 있음.**

<br/>

---

## **스코프의 종류**

| **종류**       | **설명**                                         | **예제**                      |
|--------------|--------------------------------|------------------------------|
| **전역 스코프** | 코드의 가장 바깥 영역에서 선언된 변수의 스코프. 어디서든 접근 가능. | `var globalVar = 10;` |
| **지역 스코프** | 함수 내부에서 선언된 변수의 스코프. 해당 함수 안에서만 접근 가능. | `function foo() { var localVar = 5; }` |
| **블록 스코프** | `{}` (코드 블록) 내부에서 선언된 변수의 스코프. `let`과 `const` 사용 시 적용. | `if (true) { let blockVar = 20; }` |
| **함수 레벨 스코프** | `var` 키워드로 선언된 변수는 **함수 단위**로 스코프를 가짐. | `function bar() { var functionVar = 30; }` |
| **렉시컬 스코프** | 함수가 호출된 위치가 아니라 **정의된 위치**를 기준으로 상위 스코프 결정. | `function outer() { var x = 10; function inner() { console.log(x); } }` |

---

## 스코프 체인 (Scope Chain)

~~~
함수의 중첩 구조로 인해 스코프가 계층적으로 연결되는 것
변수를 찾을 때 현재 스코프 → 상위 스코프 → 전역 스코프 순으로 탐색.
~~~

### 📌 예제
```javascript
var x = 'global x';
var y = 'global y';

function outer() {
  var z = "outer's local z";
  
  function inner() {
    var x = "inner's local x";
    console.log(x); // "inner's local x"
  }

  inner();
}

outer();
```

### 🔍 스코프 계층
```javascript
(inner)    x: "inner's local x"
  ↓
(outer)    z: "outer's local z"
  ↓
(global)   x: "global x", y: "global y"
```
💡 변수를 찾을 때:<br/>
1. 현재 스코프에서 찾고, 없으면
2. 바깥 스코프로 올라가며 찾음
3. 끝까지 없으면 에러 발생 (ReferenceError)

---

## 함수 레벨 스코프 vs 블록 레벨 스코프

~~~
var 키워드는 함수 단위로만 스코프를 가짐 (함수 레벨 스코프)
반면, let과 const는 블록({}) 단위로 스코프를 가짐 (블록 레벨 스코프).
~~~

### 📌 예제 (var는 함수 단위로 스코프 적용)
```javascript
var x = 1;

if (true) {
  var x = 10;  // 전역 변수로 간주됨
}

console.log(x); // 10 (변수가 덮어씌워짐)
```

### 📌 예제 (let, const는 블록 단위 스코프)
```javascript
let y = 1;

if (true) {
  let y = 10;  // 블록 내부에서만 유효
}

console.log(y); // 1 (전역 변수 유지)
```

💡 **var는 블록({}) 안에서도 전역 스코프로 취급되므로 주의해야 함.**

---

## 렉시컬 스코프 (Lexical Scope)
~~~
함수를 어디서 정의했는지에 따라 스코프가 결정됨 (정적 스코프).
호출 위치가 아니라 정의된 위치를 기준으로 상위 스코프를 찾음.
~~~

### 📌 예제
```javascript
var x = 1;

function foo() {
  var x = 10;
  bar(); // bar 함수 호출
}

function bar() {
  console.log(x); // ?
}

foo();
bar();
```

### 🔍 실행 결과
```bash
1
1
```

왜?<br/>
- bar()는 foo() 안에서 호출되었지만, **정의된 위치(전역 스코프)**를 기준으로 변수를 찾음.
- bar() 내부에는 x가 없으므로 전역 x(1)를 참조.

💡 자바스크립트는 렉시컬 스코프를 따름 → “함수를 어디서 정의했느냐”가 중요<br/>
(만약 동적 스코프였다면, foo() 안에서 호출했기 때문에 10이 출력될 것임)

---

## 정리

✔ 스코프란? 변수가 유효한 범위를 의미.<br/>
✔ 스코프 체인을 따라 변수를 찾음 (현재 → 상위 → 전역).<br/>
✔ var는 함수 레벨 스코프, let/const는 블록 레벨 스코프를 가짐.<br/>
✔ 렉시컬 스코프를 따르므로, “함수를 어디서 호출했느냐”가 아니라 “어디서 정의했느냐”가 중요.<br/>
