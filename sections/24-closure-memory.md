## 클로저란 ?
> 클로저는 함수가 선언될 때의 스코프를 기억하여, 함수가 생성된 이후에도 그 스코프에 접근할 수 있는 기능

```jsx
const x = 1;

function outer () {
	const x = 10;
	const inner = function () { console.log(x); };
	return inner;
}

const innerFunc = outer();
innerFunc(); // 10
```

자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다.

<br/>

![1-1](https://github.com/user-attachments/assets/1c1abfdc-bca2-4e79-bfde-17c12edc8df6)

전역에서 선언된 환경 변수인 innerFunc은 outer 함수를 값으로 선언한다.
outer 함수는 지역 변수 x와 inner함수를 선언하고 반환하며 종료된다.
inner함수를 반환받은 innerFunc를 실행하면 콘솔엔 outer의 지역 변수 x의 값인 10이 출력된다.

~~~
💡 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있는데,
   이런 중첩 함수를 클로저라고 부른다.
~~~

<br/>

단, 상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저라고 하지 않는다.   
그리고 외부 함수보다 일찍 소멸하는 중첩 함수 역시 클로저라고 하지 않는다.   

<br/>

***클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.***

<br/>

---

<br/>

## 클로저의 활용
> 클로저는 상태를 안전하게 은닉하기 위해 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

```jsx
// 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환
const counter = (function() {
  let counter = 0;
  
  return function (aux) {
    counter = aux(counter);
  }
}());

// 보조 함수
function increase(n) {
  return ++n;
}
function decrease(n) {
  return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있다.  
외부 상태 변경 등의 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용된다.  

<br/>

---

<br/>

## 캡슐화와 정보 은닉

> ***캡슐화는*** 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.  
> 이는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 ***정보 은닉***이라 한다.  

정보 은닉은 외부에 공개할 필요가 없는 일부를 공개 되지 않도록 감추어 적절치 못한 접근으로부터 객체의 사태가 변경되는 것은 방지해 정보를 보호하고, 객체 간의 결합도를 낮추는 효과가 있다.

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private
  
  // 인스턴스 메서드
  this.sayHi = function() {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  }
}

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined
```

위 예제에서 `name` 프로퍼티는 외불 공개되어 있어 자유롭게 참조 혹은 변경이 가능한 `public`한 상태이다.
하지만 `_age` 변수는 `Person` 생성자 함수의 지역 변수이므로 외부에서 참조하거나 변경할 수 없는, `private`하다.

위 예제는 변수를 선언할 때 마다 인스턴스 메서드가 중복호출 되기에

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private
}

Person.prototype.sayHi = function() {
  console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
}

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const me2 = new Person('Kim', 27);
me2.sayHi(); // Hi! My name is Kim. I am 27.
console.log(me2.name); // Kim
console.log(me2._age); // undefined
```

이렇게도 표현이 가능하지만

```jsx
me.sayHi(); // Hi! My name is Lee. I am 27.
```

`me`의 `sayHi`를 재호출했을 때 인스턴스가 동일한 상위스코프를 사용하기 때문에 `_age` 변수의 상태가 유지되지않아 결국 완전한 `private`을 구현하지는 못한다.  
하지만 최신 브라우저와 최신 노드에 이미 구현되어 있다고 한다.  

<br/>

---

<br/>

## 자주 발생하는 실수

```jsx
var funcs = [];

for(var i = 0; i < 3; i++) {
  funcs[i] = function() {return i;};
}

for(var j = 0; j < funcs.length; j++){
  console.log(funcs[j]());
}
```

위 예제가 0,1,2 를 반환할 것으로 기대했겠지만, 그렇지 않고 3이 세 번 출력된다.  
var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 현재 전역 변수다.  
전역변수는 0, 1, 2가 차례대로 할당된다.  

따라서 배열의 요소로 추가한 함수를 호출하면 전역 변수 i를 참조하여 i의 값이 3이 출력된다.  

이를 수정하려면 
1. `즉시 실행 함수` 사용
    
```jsx
var funcs = [];

for(var i = 0; i < 3; i++) {
  funcs[i] = (function(id) {
    return function() {
      return id;
    };
  }(i));
}

for(var j = 0; j < funcs.length; j++){
  console.log(funcs[j]());
}
```
    
즉시 실행 함수를 전역 변수 i에 현재 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환하고 종료된다.  
즉시 실행 함수가 반환한 함수는 funcs 배열에 순차적으로 저장된다.  
즉시 실행 함수의 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수의 상위 스코프에 존재한다.  
즉시 실행 함수가 반환한 중첩 함수는 자신의 상위 스코프를 기억하는 클로저이고, 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 그 값이 유지된다.  

<br/>
    
2. `let` 키워드를 사용해 변수 선언
    
```jsx
var funcs = [];

for(let i = 0; i < 3; i++) {
  funcs[i] = function() {return i;};
}

for(let i = 0; i < funcs.length; i++){
  console.log(funcs[j]()); // 0 1 2
}
```
    
let 키워드로 선언한 변수를 사용하면 for 문의 코드 블록이 반복 실행될 때 마다 for 문 코드 블록의 새로운 렉시컬 환경이 생성된다.  
이때 함수의 상위 스코프는 for 문의 코드 블록이 반복 실행될 때마다 식별자의 값을 유지해야한다. 

이를 위해 for문의 코드 블록이 반복 실행되기 시작되면 새로운 렉시컬 환경을 생성해 for 문 코드 블록 내의 식별자와 값을 등록한다.  
그리고 새롭게 생성된 렉시컬 환경을 현재 실행 중인 실행 컨텍스트의 렉시컬 환경으로 교체한다. 반복 실행이 종료되면 이전의 렉시컬 환경을 실행 중인 실행 컨텍스트의 렉시컬 환경으로 되돌린다.  
