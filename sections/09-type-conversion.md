## 타입 변환

### 1) **명시적 타입 변환 / 타입 캐스팅**
> 개발자가 의도적으로 값의 타입을 변환하는 것

1. **문자열 타입으로 반환**
    1. `String` 생성자 함수를 new 연산자 없이 호출하는 방법
    2. `Object.prototype.toString` 메서드를 사용하는 방법
    3. 문자열 연결 연산자를 이용하는 방법
    
2. **숫자 타입으로 반환**
    1. `Number` 생성자 함수를 `new` 연산자 없이 호출하는 방법
    2. `parseInt`, `parseFloat` 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
    3. `+` 단항 산술 연산자를 이용하는 방법
    4. `*` 산술 연산자를 이용하는 방법
    
3. **불리언 타입으로 반환**
    1. `Boolean` 생성자 함수를 `new` 연산자 없이 호출하는 방법
    2. `!` 부정 논리 연산자를 두 번 사용하는 방법

<br/>

### 2) **암묵적 타입 변환 / 타입 강제 변환**
> 개발자의 의도와는 상관없이 자바스크립트의 엔진에 의해 암묵적으로 타입이 변환되는 것

1. **문자열 타입으로 변환**
    1. 문자열 연결 연산자 (`+`)
        > 문자열 연결자의 역할은 문자열 값을 만드는 것이다.
        
        ```jsx
        1 + '2' // -> '12'
        ```
        
        위 예제처럼 피연산자 중 하나 이상이 문자열일 때 `+` 는 문자열 연결 연산자로 동작하여 1의 타입을 문자열로 암묵적으로 변환하여 문자열의 결과값을 만든다
        
    2. 템플릿 리터럴
        > 템플릿 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 압묵적 타입 변환한다
        
        ```jsx
        `1 + 1 = ${1 + 1}` // -> '1 + 1 = 2'
        ```
        
    3. 기타
        ```jsx
        // 숫자 타입
        0 + '' // -> '0'
        -0 + '' // -> '0'
        1 + '' // -> '1'
        -1 + '' // -> '-1' 
        
        // 불리언 타입
        true + '' // -> 'true'
        false + '' // -> 'false'
        
        // null 타입
        null + '' // -> 'null'
        
        // undefined 타입
        undefined + '' // -> 'undefined'
        
        // symbol 타입
        (Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string
        
        // 객체 타입
        ({}) + '' // -> '[object object]'
        Math + '' // -> '[object Math]'
        [] + '' // ''
        [10,20] + '' // -> 10,20
        (function(){}) + '' // -> 'function(){}'
        Array + '' // -> 'function Array() { [native code] }' 
        ```
        

1. **숫자 타입으로 변환**
    1. 산술연산자(`*`, `-`, `/`)
        > 산술 연사자의 역할은 숫자 값을 만드는 것이다.
        
        ```jsx
        1 - '1' // -> 0
        1 * '10' // -> 10
        1 / 'one' // -> NaN
        ```
        
        산술연산자는 모든 피연산자가 모두 숫자 타입이어야 하기 때문에 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적으로 변환한다.
        하지만 피연산자를 숫자 타입으로 변환할 수 없는 경우는 NaN이 된다
        
    2. 비교연산자
        > 비교연산자는 피연산자의 크기를 비교하므로 모든 피연산자는 숫자 타입이어야 한다
        
        ```jsx
        '1' > 0 // -> true
        ```
        
    3. `+` 단항연산자
        > 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.
        
        ~~~
        ❗ 객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다 !!
        ~~~
        
        ```jsx
        // 문자열 타입
        +'' // -> 0
        +'0' // -> 0
        +'1' // -> 1
        +'string' // -> NaN
        
        // 불리언 타입
        +true // -> 1
        +false // -> 0
        
        // null 타입
        +null // -> 0
        
        // undefined 타입
        +undefined // -> NaN
        
        // 심벌 타입
        +Symbol() // -> TypeError: Cannot convert a Symbol value to a number
        
        // 객체 타입
        +{} // -> NaN
        +[] // -> 0
        +[10, 20] // -> NaN
        +(function(){}) // -> NaN
        ```
        
    
2. **불리언 타입으로 변환**
> 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값 (거짓으로 평가되는 값)으로 구분

- Falsy로 구분되는 값들
    - false
    - undefined
    - null
    - 0, -0
    - NaN
    - ‘’ (빈 문자열)

 <br/>

---

<br/>

## 단축 평가
> 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 의미

#### 1) 논리 연산자를 사용한 단축 평가 ( `||`  /  `&&`)

- **논리합( `||` ) 연산자**
    > 논리합 연산자는 두 개의 피연산자 중 <i>하나만</i> `true`여도 `true`를 반환
- **논리곱( `&&` ) 연산자**
    > 논리곱 연산자는 두 개의 피연산자가 <i>모두</i> `true`일 때, `true`를 반환

  | 표현식               | 평가 결과         |
  |----------------------|-------------------|
  | true \|\| anything   | true             |
  | false \|\| anything  | anything         |
  | true && anything     | anything         |
  | false && anything    | false            |

🌟**유용한 상황**
- 객체를 가리키기를 기대하는 변수가 `null` 또는 `undefined` 인지 확인하고 프로퍼티를 참조할 때
- 함수 매개변수에 기본값을 설정할 때

#### 2) 옵셔널 체이닝 연산자 ( `?.` )
> 좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined`를 반환하고, 아닐 시 우항의 프로퍼티 참조를 이어간다.

#### 3) null 병합 연산자 ( `??` )
> 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다
