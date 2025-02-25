## 📌 var, let, const 키워드 비교 및 특징

JavaScript에서 변수를 선언할 때 사용하는 `var`, `let`, `const` 키워드의 차이를 정리하고,  
각 키워드의 특징과 사용 시 주의할 점을 살펴봅니다.

---

### **1️⃣ var 키워드의 문제점**

~~~
`var` 키워드는 몇 가지 문제점을 가지고 있어 최신 JavaScript에서는 `let` 또는 `const`를 사용하는 것이 권장됩니다.
~~~

#### ✅ 1. 변수 중복 선언 허용

```javascript
var x = 1;
var x = 10; // 같은 이름으로 다시 선언 가능 (의도치 않은 값 변경 위험)
console.log(x); // 10
```

> ✔ 동일한 변수명을 여러 번 선언할 수 있어, 변수 값이 의도치 않게 덮어쓰기될 위험이 있음.<br/>


#### ✅ 2. 함수 레벨 스코프

~~~
var 키워드는 함수 단위로 스코프를 가지며, if문, for문 같은 블록에서는 지역 변수가 생성되지 않음.
~~~

```javascript
var x = 1;

if (true) {
  var x = 10; // 전역 변수로 취급됨 (블록 스코프 X)
}

console.log(x); // 10 (의도치 않게 값 변경됨)
```
``` javascript
for (var i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

console.log(i); // 5 (for문 블록 밖에서도 접근 가능)
```
> ✔ var를 사용하면 블록을 무시하고 변수가 전역적으로 영향을 미치는 문제가 발생할 수 있음.<br/>

#### ✅ 3. 변수 호이스팅

~~~
• var는 선언 단계에서 자동으로 스코프의 최상단으로 끌어올려짐 (호이스팅).
• 하지만 초기화는 안 되므로, undefined가 출력됨.
~~~

```javascript
console.log(foo); // undefined
var foo = 123;
console.log(foo); // 123
```

> ✔ var 변수는 선언 전에 접근할 수 있지만, 초기화되지 않아 undefined를 반환하는 문제가 있음.<br/>

---

### 2️⃣ let 키워드

~~~
let은 var의 단점을 보완한 변수 선언 방식입니다.
~~~

#### ✅ 1. 변수 중복 선언 금지
```javascript
let x = 1;
let x = 10; // ❌ SyntaxError: Identifier 'x' has already been declared
```
> ✔ 동일한 변수를 중복 선언할 수 없음.<br/>

#### ✅ 2. 블록 레벨 스코프
~~~
let은 블록({}) 단위로 스코프를 가짐.
~~~

```javascript
let x = 1;

if (true) {
  let x = 10; // if 블록 내에서만 유효
  console.log(x); // 10
}

console.log(x); // 1 (블록 밖의 x 값 유지)
```
> ✔ var처럼 전역적으로 영향을 주지 않고, 블록 내부에서만 변수 값이 유지됨.<br/>

#### ✅ 3. 변수 호이스팅
~~~
let도 변수 호이스팅이 발생하지만, 초기화 단계가 분리되어 선언 전에 참조하면 에러 발생.
~~~

```javascript
console.log(foo); // ❌ ReferenceError: Cannot access 'foo' before initialization
let foo = 1;
console.log(foo); // 1
```
> ✔ var와 달리, 초기화 이전에는 변수를 참조할 수 없음 (일시적 사각지대, TDZ).<br/>

#### ✅ 4. 전역 객체(window)와의 관계
~~~
var로 선언한 전역 변수는 window 객체의 속성이 되지만, let은 그렇지 않음.
~~~

```javascript
var x = 1;
console.log(window.x); // 1

let y = 10;
console.log(window.y); // undefined
```
> ✔ let을 사용하면 전역 객체 오염을 방지할 수 있음.<br/>

---

### 3️⃣ const 키워드

~~~
const는 값을 재할당할 수 없는 상수 선언 방식입니다.
~~~

#### ✅ 1. 선언과 동시에 초기화 필수

```javascript
const foo = 1;
const bar; // ❌ SyntaxError: Missing initializer in const declaration
```

> ✔ const는 선언과 동시에 값을 지정해야 함.

#### ✅ 2. 재할당 금지
```javascript
const x = 1;
x = 10; // ❌ TypeError: Assignment to constant variable.
```
> ✔ const는 변수 값을 변경할 수 없음.<br/>

#### ✅ 3. 객체와 배열도 변경 가능 (불변 객체가 아님)
```javascript
const person = { name: "Lee" };
person.name = "Kim"; // 가능 (객체 내부 속성 변경)
console.log(person); // { name: "Kim" }
```
> ✔ const는 객체 자체를 변경할 수는 없지만, 내부 속성 값 변경은 가능.<br/>

---

### 🚀 var, let, const 차이점 정리

| 특징 | var | let | const |
|------|----|----|----|
| **중복 선언** | 가능 | 불가능 | 불가능 |
| **스코프** | 함수 레벨 | 블록 레벨 | 블록 레벨 |
| **호이스팅** | 선언+초기화(`undefined`) | 선언만, TDZ 발생 | 선언만, TDZ 발생 |
| **재할당** | 가능 | 가능 | 불가능 |
| **전역 객체(window)와의 관계** | 속성으로 등록됨 | 등록 안됨 | 등록 안됨 |
