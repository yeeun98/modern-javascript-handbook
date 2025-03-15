## 📘 this 키워드

> this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.  
this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
> 

**this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**

<br/>

1. **전역에서의 this**
    
```jsx
console.log(this); //window
```

<br/>
    
2. **일반 함수 내부에서의 this**
    
```jsx
function square(number) {
  console.log(this); // window
  return number * number;
}
```

<br/>
    
3. **메서드 내부에서의 this**
    
```jsx
const person = {
  name: 'Lee',
  getName() {
    console.log(this); // 메서드는 호출한 객체를 가리킴 {name: 'Lee', getName: f}
    return this.name;
  }
}
```

<br/>

4. **생성자 함수 내부에서 this**
    
```jsx
function Person(name) {
  this.name = name;
  console.log(this); // Person {name: 'Lee'}
}
```

---

## 함수 호출 방식과 this 바인딩

1. **일반 함수 호출**
    
> 전역 객체가 바인딩된다

```jsx
foo(); //window
```

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this : ", this); // {value: 100, foo: f}
    console.log("foo's this.value : ", this.value); // 100
  
    // 콜백 ㅎ마수의 내부에서 this는 전역 객체가 바인딩 됨
    setTimeout (function(){
      console.log("callback's this : ", this); // this
      console.log("callback's this.value : ", this.value); // 100
    }()};
  }
}
```
    
**이처럼 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함)의 내부에서 this는 전역 객체가 바인딩된다.**

<br/>

2. **메서드 호출**
> this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩이 된다

```jsx
const person = {
  name: 'Lee',
  getName() {
    return this.name;
  }
}

console.log(person.getName()); // Lee

const anotherPerson = {
  name: 'Kim'
}
anotherPerson.getName = person.getName;

console.log(anotherPerson.getName); // Kim
```

<br/>

3. **생성자 함수 호출**
    
> this는 생성자 함수가 (미래에 생성할 인스턴스가 바인딩된다.)

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function() {
    return 2 + this.radius;
  }
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

<br/>
    
4. **Function.prototype.apply/call/bind 메서드에 의한 간접 호출**

  a. apply
      
  ```jsx
  /**
  * @param thisArg - this로 사용할 객체
  * @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
  * @returns 호출할 함수의 반환값
  */
  function.prototype.apply(thisArg[, argsArray])
  ```

  b.   call
  
  ```jsx
  /**
  * @param thisArg - this로 사용할 객체
  * @param args1, args2 - 함수에게 전달할 인수 리스트
  * @returns 호출할 함수의 반환값
  */
  function.prototype.call(thisArg[, args1[, args2[, ...]]])
  ```
    
  ❗ apply, call은 본질적으로는 함수를 호출하는 기능을 한다.
  
  ```jsx
  function getThisBinding() {
    return this;
  }
  
  const thisArg = { a: 1 };
  
  console.log(getThisBinding()); // window
  
  // getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
  console.log(getThisBinding.apply(thisArg)); // { a: 1 }
  console.log(getThisBinding.call(thisArg)); // { a: 1 }
  ```
    
  c.   bind
  
  > apply와 call 메소드와 달리 함수를 호출하지 않고 this로 사용할 객체만 전달한다.
  
  ```jsx
  function getThisBinding() {
  	return this;
  }
  
  const thisArg = { a: 1 };
  
  console.log(getThisBinding()); // window
  
  // bind는 함수에 this로 사용할 객체를 전달하지만 함수 호출은 하지 않는다.
  console.log(getThisBinding.bind(thisArg)); // getThisBinding
  // bind는 함수를 호출하지 않으므로 명시적으로 호출해야 한다.
  console.log(getThisBinding.bind(thisArg)()); // { a: 1 }
  ```
  
  > 메서드 내의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결할 때 유용
  
  ```jsx
  const person = {
  	name: 'Lee',
  	foo(callback) {
  		setTimeout(callback, 100);
  	}
  }
  
  person.foo(function() {
  	console.log('Hi! my name is ${this.name}.'); // Hi! my name is .
  });
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 window.name과 같다.
  ```
  
  위 처럼 foo함수 내부에서의 this와 중첩 함수 내부에서의 this는 다를 때 bind를 통해 this를 일치시킨다.
  
  ```jsx
  const person = {
  	name: 'Lee',
  	foo(callback) {
  		setTimeout(callback.bind(this), 100);
  	}
  }
  
  person.foo(function() {
  	console.log('Hi! my name is ${this.name}.'); // Hi! my name is Lee.
  });
  ```
