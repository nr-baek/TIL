# this

실행 컨텍스트의 수집 정보는 VariableEnvironment, LexicalEnvironment, ThisBinding로 구성되어있고 그 중 ThisBinding에는 this로 지정된 객체가 저장된다.

실행 컨텍스트가 활성화 될 때(함수가 호출 되는 순간) this를 바인딩한다!!(this는 함수가 호출될 때 결정된다.) = 함수를 어떤식으로 호출했느냐에 따라서 this가 달라진다.(동적으로 바인딩 된다.)

## 상황에 따라 달라지는 this

*this는 함수를 호출하는 방식에 따라 달라진다.*

### 전역 공간에서의 this

- (브라우저 콘솔에서)window / (node.js에서)global
- 전역 공간에서의 this는 전역객체를 가리킨다.

### 함수 호출시의 this

- (브라우저 콘솔에서)window / (node.js에서)global
- 함수로서 호출시에 this는 전역객체를 가리킨다.
- 함수는 전역객체의 메소드라고 생각하면 편하다.
- 내부 함수 안에서의 this도 전역객체를 가리킨다.
  
```javascript
  // 1. 전역 공간에서 호출한 함수의 this
  function a() {
    console.log(this);
  }
  a(); // 전역공간에서 호출했기 때문에 전역객체가 출력된다.(호출한 대상이 전역공간)

  // 2. 함수 내부에서의 this
  function b() {
    function c() {
      console.log(this);
    }
    c();  // b함수 내부에서 실행되는 함수이지만 this는 전역객체.
    // 함수로서 호출됐으면 언제나 this는 전역객체를 바라본다.
  }
  b();  // 전역 공간에서 호출(this는 전역객체)

  // 3. 메소드의 내부함수 this
  var d = {
    e: function() {
      function f() {
        console.log(this);
      }
      f(); // e가 객체의 프로퍼티로 할당되어 메소드이지만 그 내부의 함수가 함수로서 호출이 되었기 때문에 this는 전역객체를 바라본다.
    }
  }
  d.e();  // 전역객체(window/global)가 출력된다.
```

#### 내부함수에서의 this를 우회하는 방법

this를 우회하지 않고 그냥 작성했을 때

```javascript

var a = 10;
var obj = {
  a: 20,
  b: function() {
    console.log(this.a);  // 20 출력

    function c() {
      console.log(this.a);  // 10 출력
    }
    c();
  }
}
obj.b();
// obj.b메서드 안의 this.a가 obj.a를 가리키는데
// obj.b메서드의 내부함수에서의 this.a는 window.a를 가리킨다.
// 내부함수c는 c();와 같이 함수로서 호출됐기 때문에 this는 전역객체가 됨.
// 내부함수c의 this.a가 obj.a를 바라보지 않는다.
```

this를 우회하는 방법으로 작성했을 때

```javascript
// this를 변수에 담아서 내부함수에 쓰는 방법

var a = 10;
var obj = {
  a: 20,
  b: function() {
    var self = this;  // 현재 컨텍스트상의 this를 변수에 담는다.
    console.log(this.a); // 20 출력

    function c() {
      console.log(self.a);  //내부함수에서 this대신 변수를 쓴다.  20 출력
    }
    c();
  }
};
obj.b();
```

### 함수를 메소드로서 호출했을 시의 this

이 때의 this는 **메소드 호출 주체**(메소드명 '.'앞의 객체)
  
```javascript
  // 메소드로서 호출한 경우 1
  var a = {
    b: function() {
      console.log(this);
    }
  }
  a.b(); // a객체가 출력된다. (this는 a객체)
  ```

  ```javascript
  // 메소드로서 호출한 경우 2
  var a = {
    b: {
      c: function() {
        console.log(this);
      }
    }
  };
  a.b.c(); // a.b가 this가 되어 b객체가 출력이 된다.
```

### 콜백함수에서의 this

- 기본적으로는 함수의 this와 같다.  
- 제어권을 가진 함수가 callback의 this를 명시한 경우 그에 따른다.
- 개발자가 this를 바인딩한 채로 callback을 넘기면 그에 따른다.

#### call, apply, bind 메소드_ 명시적인 this 바인딩

call, apply, bind메소드들로 this에 별도의 대상을 명시적으로 바인딩할 수 있다.

##### call

call메소드는 메소드의 호출 주체인 함수를 즉시 실행하도록 명령한다.  
call메소드를 이용하면 임의의 객체를 this로 지정할 수 있다.
첫번째 인자에 this로 지정할 객체를 넣고 나머지 모든 인자들을 호출할 함수의 매개변수로 지정한다.

##### apply

apply메소드는 call과 기능적으로 동일하다.
첫번째 인자에 this로 지정할 객체를 넣고 두번째 인자에 배열을 넣어 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다.

##### bind

call메소드와 비슷하지만 즉시 호출하지는 않고 넘겨받은 this 및 인수들을 바탕으로 새로운 함수를 *반환하기만 하는 메소드*이다.  
(this를 지정한 새로운 함수를 반환)

```javascript
function a(x, y, z) {
  console.log(this, x, y, z);
}

var b = {
  c: 'eee'
};

a.call(b, 1, 2, 3); // b객체 1 2 3 출력

a.apply(b, [1, 2, 3]); // b객체 1 2 3 출력

var c = a.bind(b);
c(1, 2, 3); // b객체 1 2 3 출력

var d = a.bind(b, 1, 2);
d(3); // b객체 1 2 3 출력

// 각각 this값으로 b객체가 출력됨

```

콜백함수에서의 this예시

```javascript
// 함수 내부에서 그냥 함수로서 실행된 경우

var callback = function() {
  console.log(this);
};
var obj = {
  a: 1,
  b: function(cb) {
    cb();  //전역객체 출력
  }
};
obj.b(callback);
// obj.b()메소드로서 호출됐지만 결국 cb()는 함수로서 호출되었기때문에
// 전역객체가 출력이 된다.
```

```javascript
// this를 명시적으로 바인딩한 경우

var callback = function() {
  console.log(this);
};
var obj = {
  a: 1,
  b: function(cb) {
    cb.call(this); // b실행 컨텍스트상의 this를 바인딩함
                   // 따라서 obj가 출력이 된다.
  }
};
obj.b(callback);
```

콜백함수 내부의 this는 지정하는 것에 따라 달라진다.

### 생성자 함수 호출시의 this

new명령어와 함께 함수를 호출하면 해당 함수가 생성자로서 동작하는데,  
어떤 함수가 생성자 함수로서 호출된 경우 내부에서의 this는 곧 새로 만들 구체적인 인스턴스 자신이 된다.  

```javascript
var Cat = function (name, age) {
  this.bark = '애용';
  this.name = name;
  this.age = age;
};

var coco = new Cat('코코', 7);
var nabi = new Cat('나비', 3);
console.log(coco, nabi);
/* 결과
Cat {bark: '애용', name: '코코', age: 7}
Cat {bark: '애용', name: '나비', age: 3}
*/
```
