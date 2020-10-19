> [poiemaweb.com](https://poiemaweb.com) 참고

# 클래스(class)

클래스는 생성자 함수처럼 프로토타입 기반의 인스턴스를 생성하는 함수다.  
단, 클래스는 생성자 함수와 매우 유사하게 동작하지만 다음과 같이 몇 가지 차이가 있다.  

1. 생성자 함수는 new연산자 없이 호출하면 일반 함수로서 호출되지만 클래스는 new 연산자 없이 호출하면 에러가 발생한다.  
2. 클래스는 생성자 함수와 다르게 상속을 지원하는 extends와 super키워드를 제공한다.
3. 클래스는 마치 let,const키워드처럼 호이스팅이 발생하지 않는 것처럼 동작한다.
    - 때문에 클래스는 정의 이전에 참조할 수 없다.
4. 생성자 함수와 다르게 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행된다.
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다.(열거되지 않음)

## 클래스 정의

클래스는 class키워드를 사용하여 정의한다.클래스 이름은 생성자 함수와 마찬가지로 파스칼 케이스를 사용하는 것이 일반적이다.  
클래스는 함수이기 때문에 따라서 일급 객체이다. 값처럼 사용 가능하며 변수에 담을 수 있기 때문에 표현식으로도 정의가 가능하다.  

```javascript
class Person {}

// 클래스는 함수다.
console.log(typeof Person); // function
```

클래스의 몸체에는 0개 이상의 메서드만 정의할 수 있는데, 그 메서드는 constructor메서드, 프로토타입 메서드, 정적 메서드. 이렇게 세 가지가 있다.  

```javascript
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log('Hello!');
  }
}

// 인스턴스 생성
const me = new Person('Lee');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

## 인스턴스 생성

클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다.  
클래스의 존재 목적은 인스턴스를 생성하는 것이기 때문에 반드시 new연산자를 붙여서 호출하도록 해야한다.

```javascript
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

클래스도 함수처럼 클래스 이름이 아닌 식별자를 사용해서 호출해야 한다.  
이는 기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가능하기 때문이다.

## 메서드

클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다. (메서드를 아무것도 작성 하지 않아도 에러가 발생하지 않으며 이 때 생성자 함수는 빈 객체_인스턴스만을 생성한다)  

### constructor

constructor는 **인스턴스를 생성하고 초기화하기 위한 특수한 메서드**다.
(constructor는 이름을 변경할 수 없다)  
constructor 내부에서 this에 추가하 프로퍼티는 인스턴스의 프로퍼티가 된다.  

```javascript
// 클래스
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다.  
그런데 constructor는 모양만 메서드지 사실 메서드로 해석되지는 않는다.  
(클래스가 평가되어 생성된 함수 객체나 클래스가 생성한 인스턴스를 보면 constructor메서드가 보이지 않는 것을 알 수 있다.)
constructor는 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다. 다시 말해, 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.  
주의할 점은 constructor는 클래스 내에 최대 한 개만 존재할 수 있다는 것이다.  만약 클래스가 2개 이상의 constructor를 포함하면 문법 에러가 발생한다.  
constructor는 생략할 수도 있는데 만약 생략을 하면 클래스에는 내부에 constructor가 암묵적으로 정의되고 빈 constructor에 의해 빈 객체를 생성한다.  

```javascript
class Person {
  constructor(name, address) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

// 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person('Lee', 'Seoul');
console.log(me); // Person {name: "Lee", address: "Seoul"}
```

프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.  
인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 위의 코드처럼 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다.  
constructor는 별도의 반환문을 갖지 않아야 한다.  
생성자 함수처럼 클래스는 암묵적으로 this(인스턴스)를 반환하기 떄문이다.  
만약 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return문에 명시한 객체가 반환된다.(명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적인 this가 반환된다)  
따라서 constructor내부에서 return문은 반드시 생략해야 한다.  

### 프로토타입 메서드

생성자 함수를 사용하여 인스턴스를 생성하는 경우, 프로토타입 메서드를 생성하기 위해서는 다음과 같이 명시적으로 프로토타입에 메서드를 추가해야 하지만 클래스 몸체에서 정의한 메서드는 기본적으로 프로토타입 메서드가 된다.  

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 되어 프로토타입 메서드를 상속받아 사용할 수 있다.  

### 정적 메서드

정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.(인스턴스를 통해서는 호출할 수 없다.)  
생성자 함수의 경우 정적 메서드를 생성하기 위해서는 다음과 같이 명시적으로 생성자 함수에 메서드를 추가해야 한다.  
그러나 클래스에서는 메서드에 **static** 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다.  

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log('Hi!');
  }
}

Person.sayHi(); // Hi!
```

클래스 몸체 내부에 static 키워드를 붙여 생성한 정적 메서드는 클래스에 바인딩된 메서드가 된다.  
정적 메서드는 프로토타입 메서드처럼 인스턴스로 호출하지 않고 클래스로 호출한다.  
정적 메서드는 인스턴스의 프로토타입 체인 상에 존재하지 않기 때문에 인스턴스로 클래스의 메서드를 상속받을 수 없다. 때문에 클래스로만 호출이 가능하다.  

### 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 메서드사이의 콤마가 필요 없다.  
3. 암묵적 strict모드 실행
4. 프로퍼티 어트리뷰트[[Enumerable]]의 값이 false다.
    - 열거하지 못한다.
5. new 연산자와 함께 호출할 수 없다.
    - [[Construct]]내부슬롯을 갖지 않는 non-constructor이다.

## 클래스의 인스턴스 생성 과정

### 1. 인스턴스 생성과 this바인딩

new 연산자와 함께 클래스를 호출하면 constructor메서드의 내부 코드가 실행되기 앞서 암묵적으로 빈 객체가 생성이 되는데 이는 클래스가 생성한 인스턴스이다.  
이 때 인스턴스의 프로토타입으로 클래스의 프로토타입 프로퍼티가 가리키는 객체가 설정된다. 그리고 인스턴스는 this에 바인딩된다.  

### 2. 인스턴스 초기화

constructor의 내부 코드가 실행된다.  
this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 받은 초기값으로 인스턴스의 프로퍼티 값을 초기화한다.  

### 3. 인스턴스 반환

클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.(때문에 return문은 반드시 생략해야한다.)

## extends 키워드

상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.  

```javascript
// 수퍼(파생/부모)클래스
class Base {}

// 서브(파생/자식)클래스
class Derived extends Base {}
```

상속을 통해 확장된 클래스를 서브클래스(subclass)라 부르고, 서브클래스에게 상속된 클래스를 수퍼클래스(superclass)라 부른다. 서브클래스를 파생 클래스(derived class) 또는 자식 클래스(child class), 수퍼클래스를 베이스 클래스(base class) 또는 부모 클래스(parent class)라고 부르기도 한다.
**extends 키워드의 역할은 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것이다.** 클래스도 프로토타입을 통해 상속 관계를 구현한다.
