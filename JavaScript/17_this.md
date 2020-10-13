> [poiemaweb.com](https://poiemaweb.com) 참고

# this

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)이다.

## this 키워드

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.  
동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 하는데 그러려면 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**  

### this 바인딩

바인딩이란 식별자와 값을 연결하는 과정을 의미하는데, this 바인딩은 this(키워드로 분류되지만 식별자 역할을 한다)와 this가 가리킬 객체를 연결하는 것을 의미한다.

## 함수 호출 방식과 this바인딩

this바인딩은 함수 호출 방식에 따라(함수가 어떻게 호출되었는지에 따라) 동적으로 결정된다.  

### 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩된다.  

```javascript
function foo() {
  console.log("foo's this: ", this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar(); // 일반 함수로 호출했기 때문에 this에는 전역 객체가 바인딩된다.
}
foo();
```

메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩되고, 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 개체가 바인딩된다.  
**어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.**

### 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체(메서드를 소유한 객체가 아닌)가 바인딩된다.  

```javascript
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.  

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.  

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

생성자 함수는 new연산자 없이 일반함수로 호출할 수도 있는데 이때 함수 내부의 this는 전역객체가 바인딩되며  
return값은 undefined가 된다.  

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드다. 즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.  

#### Function.prototype.apply

this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.  

```javascript
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray])
```

apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.

#### Function.prototype.call

this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.

```javascript
/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call (thisArg[, arg1[, arg2[, ...]]])
```

호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.  

apply와 call 메서드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다. arguments 객체는 배열이 아니기 때문에 Array.prototype.slice 같은 배열의 메서드를 사용할 수 없으나 apply와 call 메서드를 이용하면 가능하다.

#### Function.prototype.bind 메서드

apply와 call 메서드와 달리 함수를 호출하지 않고 this로 사용할 객체만 전달한다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 함수에 this로 사용할 객체를 전달한다.
// bind 메서드는 함수를 호출하지는 않는다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

|함수 호출 방식|this바인딩|
|:---:|:---:|
|일반 함수 호출|전역 객체|
|메서드 호출|메서드를 호출한 객체|
|생성자 함수 호출|생성자 함수가(미래에)생성할 인스턴스|
|Function.prototype.apply/call/bind 메서드에 의한 간접 호출|Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체|
