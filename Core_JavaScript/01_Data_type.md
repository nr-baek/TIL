# 데이터 타입

자바스크립트의 기본형 타입과 참조형 타입의 차이

## 데이터 타입의 종류

자바스크립트의 데이터 타입에는 크게 두 가지가 있다.

- Primitive type
  - Number
  - String
  - Boolean
  - null
  - undefined
  - Symbol

- Reference type
  - Object
    - Array
    - Function
    - Date
    - RegExp
    - Map, WeakMap
    - Set, WeakSet

## 데이터가 처리되는 과정

자바스크립트의 데이터가 처리되는 과정 살펴보기

### 메모리와 데이터

컴퓨터는 0과 1을 사용하는 2진수로 모든것을 표현하며 저장한다.  
최소 단위인 비트(bit)는 1 혹은 0 하나만 담을 수 있으며 8개의 비트가 모이면 1바이트(byte)가 된다.
메모리는 수많은 비트들로 구성되어있는데, 데이터가 메모리에 저장될 때 컴퓨터는 데이터형에 맞는 비트 수 만큼 메모리의 공간을 확보한 후 저장한다.  
그리고 모든 데이터는 바이트 단위의 *식별자*, `메모리 주솟값`을 통해 서로 구분하고 연결할 수 있다.  

### 식별자와 변수

'식별자'와 '변수' 단어의 미묘한 차이

- 변수
  - 변할 수 있는 데이터

- 식별자
  - 어떤 데이터를 식별하는 데 사용하는 이름
  - 즉, 변수명

``` javascript
var a; //undefined

// 변할 수 있는 데이터를 만든다.
// 이 데이터의 식별자는 a로 한다.

a = 'abc';
// 변수 a에 문자열데이터 'abc'할당.
```

변수란 결국 **변경 가능한 데이터가 담길 수 있는 공간**이다.
위의 a공간에 숫자를 담았다가 문자를 담는 등의 작업을 할 수 있다.

### 변수 선언과 데이터할당의 동작원리

`var a = 'abc'`의 과정을 메모리 영역과 함께 동작을 살펴본다.  
(메모리상 변수영역과 데이터영역이 따로 분리되어있는 것은 아니고 이해를 위해 구분되게 표시한것임)

<span style="color:gray">[변수영역]</span>
|주소|1001|1002|1003|1004|
|---|---|---|---|---|
|데이터|   |   |이름: a </br> 값: <span style="color:red">@5004</span>|   |

<span style="color:gray">[데이터영역]</span>
|주소|5001|5002|5003|5004|
|---|---|---|---|---|
|데이터|   |   |    |'abc'|

1. 변수 영역에서 빈 공간(@1003)을 확보한다.
2. 확보한 공간의 식별자를 a로 지정한다.
3. 데이터영역의 빈 공간(@5004)에 문자열 'abc'를 저장한다.
4. 변수 영역에서 a라는 식별자를 검색한다.
5. 앞서 저장한 문자열의 주소(@5004)를 @1003의 공간에 대입한다.

효율적으로 데이터의 변환을 처리하기 위해 변수와 데이터를 별도의 공간에 나누어 저장한다.

### 데이터 변환에 대한 메모리 영역의 변화

```javascript
var a;
a = 'abc';

a = 'abcdef';
// 변수a에 문자열데이터 'abcdef'할당.
```

<span style="color:gray">[변수영역]</span>
|주소|1001|1002|1003|1004|
|---|---|---|---|---|
|데이터|   |   |이름: a </br> 값:<span style="color:red">@5005</span>|   |

<span style="color:gray">[데이터영역]</span>
|주소|5002|5003|5004|5005|
|---|---|---|---|---|
|데이터|   |    |'abc'|'abcdef'|

이렇게 기존 문자열에 변환을 가하면 컴퓨터는 무조건 데이터 영역에 새로운 공간을 만들어서 문자열을 저장하고 그 주소를 변수 공간에 연결한다.  
(기존데이터 @5004는 자신의 주소를 저장하는 변수가 하나도 없게되어 가비지컬렉터에 의해 제거된다.)  

또한 이렇게 변수 영역과 데이터 영역을 분리하면 중복된 데이터에 대한 처리 효율이 높아진다.  
(=같은 데이터에 대해 새로 메모리 영역을 차지하지 않고 주소만 재사용하면 되니까)

## 기본형 데이터와 참조형 데이터

### 불변값

기본형 데이터인 Number, String, null, undefined, Symbol은 모두 불변값이다.
(불변값과 상수는 다른 개념이니 주의)
불변값에서 '불변'한다는 의미는 *메모리의 데이터 영역에서 차지한 그 값*이 변하지 않는다는 것이다.

```javascript
// 문자열과 숫자를 이용한 예시
var a = 'abc';
a = a + 'def';
// 'abc'가 'abcdef'로 바뀌는것이 아니고 메모리상 'abc'는 그대로 있고
// 새로운 문자열 'abcdef'를 만들어 그 주소를 a에 저장.
// 'abc'와 'abcdef'는 완전히 별개의 데이터이다!

var b = 5;
var c = 5;
b = 7;
// 일단 데이터 영역에서 5를 찾고 없으면 공간을 만들어 저장한다.
// 그 주소를 b에 저장.
// c에도 5를 할당하려하니 이미 만들어놓은 공간의 주소를 재활용함.
// b에 값을 7로 바꾸기위해 기존의 7이 있는지 찾아보고 없으면 새로 만들어서 저장.
```

이처럼 데이터 영역의 값을 변경할 수는 없다.  
다만 다른값의 데이터영역 공간을 새로 만드는 동작이 이루어진다.  
이것이 불변값의 성질이며, 한 번 (데이터영역에)만들어진 값은 가비지 컬렉팅을 당하지 않는 한 변하지 않는다.  

### 가변값

참조형 데이터의 기본적인 성질은 가변값인 경우가 많다.  
(하지만 설정에 따라 다른경우와 불변값으로 활용하는 방안이 있다.)  

```javascript
var obj1 = {
  a: 1,
  b: 'bbb'
};
```

<span style="color:gray">[변수영역]</span>
|주소|1001|1002|1003|1004|
|---|---|---|---|---|
|데이터|   |이름: obj1 </br> 값:<span style="color:red"> @5001</span>||   |

<span style="color:gray">[데이터영역]</sapn>
|주소|5001|5002|5003|5004|
|---|---|---|---|---|
|데이터|@7103 ~ ?|    |1|'bbb'|

<span style="color:gray">[객체 @5001의 변수영역]</span>
|주소|7103|7104|7105|7106|
|---|---|---|---|---|
|데이터|이름: a </br> 값:<span style="color:red"> @5003</span>|이름: b </br> 값:<span style="color:red"> @5004</span>    |||

기본형 데이터와의 차이는 '객체의 변수(프로퍼티) 영역'이 별도 존재한다는 점.  
데이터 영역에 저장된 값은 모두 불변값이다.  
(데이터 영역의 값은 바꿀 수 없고 새로 만드는 동작만 있기 때문)  
그러나 변수에는 다른 값을 얼마든지 대입할 수 있다.  
때문에 참조형 데이터는 불변하지 않는 값(가변값)이라고 한다.

```javascript
var obj1 = {
  a: 1,
  b: 'bbb'
};

obj1.a = 2;
```

obj의 a 프로퍼티에 숫자 2를 할당하려고 하면 데이터 영역에서 숫자 2를 검색한 후 없으면 빈 공간에 저장한다.  
그리고 그 주소값을 객체 변수영역의 a 공간 값에 저장한다.
참조형 데이터의 프로퍼티에 다시 참조형 데이터를 할당하는 중첩 객체의 경우도 마찬가지이다.

### 기본형 데이터와 참조형 데이터의 차이

#### 변수 복사 비교

```javascript
var a = 10;
var b = a; // 기본형 복사

var obj1 = { c: 10, d: 'ddd' };
var obj2 = obj1; // 참조형 복사
```

변수b에 변수a의 값을 복사한다.
변수obj2에 변수obj1의 값을 복사한다.

<span style="color:gray">[변수영역]</span>
|주소|1001|1002|1003|1004|...|
|---|---|---|---|---|---|
|데이터|이름: a </br> 값:<span style="color:red"> @5001</span>|이름: b </br> 값:<span style="color:red"> @5001</span>|이름: obj1 </br> 값:<span style="color:red"> @5002</span>|이름: obj2 </br> 값:<span style="color:red"> @5002</span>||

<span style="color:gray">[데이터영역]</sapn>
|주소|5001|5002|5003|5004|...|
|---|---|---|---|---|---|
|데이터|10|@7103 ~ ?|'ddd'|||

<span style="color:gray">[객체 @5001의 변수영역]</span>
|주소|7103|7104|...|...|
|---|---|---|---|---|
|데이터|이름: c </br> 값:<span style="color:red"> @5001</span>|이름: d </br> 값:<span style="color:red"> @5003</span>    |||

변수a의 데이터 주소값 @5001을 변수b의 값으로 대입하고  
변수 obj1의 데이터 주소값 @5002를 obj2의 값으로 대입한다.
할당하는 과정에서는 큰 차이가 있지만 변수를 복사하는 과정은 기본형 데이터와 참조형 데이터 모두 같은 주소를 바라보게 되는 점에서 동일하다.

#### 변수 복사 후 값 변경 결과

```javascript
var a = 10;
var b = a;
var obj1 = { c: 10, d: 'ddd' };
var obj2 = obj1;

b = 15;
obj2.c = 20;
```

<span style="color:gray">[변수영역]</span>
|주소|1001|1002|1003|1004|...|
|---|---|---|---|---|---|
|데이터|이름: a </br> 값:<span style="color:red"> @5001</span>|이름: b </br> 값:<span style="color:red"> @5004</span>|이름: obj1 </br> 값:<span style="color:red"> @5002</span>|이름: obj2 </br> 값:<span style="color:red"> @5002</span>||

<span style="color:gray">[데이터영역]</sapn>
|주소|5001|5002|5003|5004|5005|
|---|---|---|---|---|---|
|데이터|10|@7103 ~ ?|'ddd'|15|20|

<span style="color:gray">[객체 @5001의 변수영역]</span>
|주소|7103|7104|...|...|
|---|---|---|---|---|
|데이터|이름: c </br> 값:<span style="color:red"> @5005</span>|이름: d </br> 값:<span style="color:red"> @5003</span>    |||

기본형 데이터를 복사한 변수b의 값을 바꿨더니 변수b의 데이터 주소값이 변경되었다.  
(10을 가리키던 @5001에서 20을 가리키는 @5004로 바뀜)  
반면에 참조형 데이터를 복사한 변수obj2의 프로퍼티의 값을 바꿨더니 obj2의 데이터 주소값은 그대로 @5002를 가리키며 여전히 obj1과 같은 객체를 바라보는 상태이다.

`a !== b`  
`obj1 === obj2`  

이 결과가 기본형과 참조형 데이터의 가장 큰 차이점이다.  
기본형은 주솟값을 복사하는 과정이 한 번만 이뤄지고, 참조형은 한 단계를 더 거치게 된다는 차이다.
그러나 obj2에 새로운 객체를 할당함으로써 값을 직접 변경하면!  
메모리의 데이터 영역의 새로운 공간에 새 객체를 생성하고 그 주소를 변수영역의 obj2위치에 저장한다.  
그럼 기본형처럼 값이 달라지게 된다.  

```javascript
var obj1 = { c: 10, d: 'ddd' };
var obj2 = obj1;

obj2 = { c: 20, d: 'ddd'}; // 새로운 객체를 할당함
// 내부 프로퍼티의 값만 변경한 것이 아닌 객체 자체를 변경!
```

***즉, 참조형 데이터 자체를 변경할 경우가 아니라 그 내부의 프로퍼티를 변경할 때만 가변값이 되는 것이다.***

### 불변 객체

가변값인 참조형 데이터는 위험할 수 있다.

```javascript
var obj1 = {
  a: 10,
  b: "ddd"
};

var obj2 = obj1;

obj2.a = 20;

console.log(obj1.a); //20
console.log(obj2.a); //20

// obj1의 프로퍼티 값도 20으로 변경되었다.
```

예를들어 위와 같은 경우 obj2 변수에 obj1을 할당하고 obj2의 프로퍼티를 변경하면  
obj1은 obj2와 같은 객체를 바라보고 있기 때문에 obj1의 프로퍼티 값도 달라지게 된다.
만약 의도하지 않은 결과라면 위험한 코드가 된다.  

*값으로 전달받은 객체에 변경을 가하더라도 원본 객체는 변하지 않아야 하는 경우* 불변 객체가 필요하다.

```javascript
var user = {
  name: 'Koko',
  age: 10
};

var changeName = function (user, newName) {
  var newUser = user;
  newUser.name = newName;
  return newUser;
};

var user2 = changeName(user, 'Park');

console.log(user.name, user2.name); // Park Park
console.log(user === user2); // true
```

가변성에 따른 문제점을 보여주는 또다른 코드이다.  
user와 user2가 서로 다른 객체여야 하는데 같은 객체를 가리키고 있기 원본의 프로퍼티값도 바뀌는 결과가 나왔다.  
changeName함수에 파라미터로 들어가는 객체와 return되는 값이 서로 다른 객체를 바라보게 만들어야 한다.  

<span style = "color:gray">[ 가변성 문제를 해결하기 위한 시도 ]</span>

```javascript

var user = {
  name: "Koko",
  age: 10
};

var copyObject = function(obj) {
  var result = {};

  for (var key in obj) {
    result[key] = obj[key];
  }
  return result;
};

var user2 = copyObject(user);
user2.name = "park";

console.log(user.name, user2.name); // Koko park
console.log(user === user2); // false
```

user 객체의 프로퍼티들을 복사해서 새로운 객체를 반환하는 함수 copyObject를 사용하여 user와 user2가 서로 다른 객체를 바라보게 만들었다.

그.러.나. 이 또한 문제가 되는데,
만약 프로퍼티에 참조형 데이터가 할당된 중첩 객체인 경우에 copyObject함수를 사용하면 '얕은 복사'가 되어 참조형 데이터가 저장된 프로퍼티를 복사할 때 그 주솟값만 복사가 되기 때문에 또다시 사본을 변경하면 원본이 변경되는 상황이 된다.  

바로 아래 단계의 값만 복사하는 방법을 *얕은 복사*라 하고, 내부의 모든 값들을 하나한 찾아서 전부 복사하는 방법을 *깊은 복사*라고 한다.  

결국 객체 내부의 모든 값을 복사해서 완전히 새로운 데이터를 만들고자 할 때는, 객체의 프로퍼티 중에서 그 값이 기본형 데이터일 경우에는 그대로 복사를 하면 되지만 참조형 데이터는 다시 그 내부의 프로퍼티들을 복사해야한다.  
참조형 데이터가 있을 때마다 재귀적으로 이 과정을 수행하면 비로소 깊은 복사가 되는 것이다.

<span style = "color:gray">[ 가변성 문제를 해결하기 위한 시도_깊은 복사의 방법 보완 ]</span>

```javascript
var copyObjectDeep = function(obj) {
  var result = {};
  if (typeof obj === "object" && obj !== null) {
    for (var prop in obj) {
      result[prop] = copyObjectDeep(obj[prop]);  // 함수를 재귀적으로 호출하여 프로퍼티의 값이 객체인지 확인하고 그 안까지 복사한다.
    }
  } else {
    result = obj;
  }
  return result;
};

var obj = {
  a: 1,
  b: {
    c: null,
    d: [1, 2]
  }
};

var obj2 = copyObjectDeep(obj);
obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj); // { a: 1, b: { c: null, d: [1, 3] }}
console.log(obj2); // { a: 3, b: { c: 4, d: { 0: 1, 1: 2 } }}
```

## undefined와 null

undefined와 null은 의미도 다르고 사용하는 목적 또한 다르다.

### undefined

자바스크립트 엔진이 undefined를 반환하는 경우

1. 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때
2. 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때
3. return문이 없거나 호출되지 않는 함수의 실행 결과

```javascript
var a;
console.log(a); // undefined

var obj = { a: 1 };
console.log(obj.a); // 1
console.log(obj.b); // undefined
// console.log(b); ReferenceError: b is not defined

var func = function() {};
var c = func();
console.log(c); //undefined
```

#### undefined와 배열

값을 대입하지 않은 경우에 대해 배열의 경우에는 특이한 동작을 확인할 수 있다.

```javascript
var arr1 = [];
arr1.length = 3;
console.log(arr1); // [empty x 3]

var arr2 = new Array(3);
console.log(arr2); // [empty x 3]

var arr3 = [undefined, undefined, undefined];
console.log(arr3); // [undefined, undefined, undefined]
```

[empty x 3] 은 배열에 3개의 빈 요소를 확보했지만 확보된 각 요소에는 어떤값도(심지어 undefined조차도) 할당돼 있지 않음을 의미한다.  
new 연산자와 함께 Array생성자 함수를 호출했을때도 결과는 같다.  
그러나 리터럴 방식으로 배열을 생성하면서 각 요소에 undefined를 부여하면 [undefined, undefined, undefined]와 같이 결과가 나타난다.  
이처럼 '비어있는 요소'와 'undefined를 할당한 요소'는 출력 결과부터도 다르며, '비어있는 요소'는 순회와 관련된 배열 메서드들의 순회 대상에서 제외된다.  
(각 메서드들이 비어있는 요소에 대해서 어떠한 처리도 하지 않고 건너 뛴다.)  

***배열도 객체다.***

배열은 객체와 마찬가지로 특정 인덱스에 값을 지정할 때 비로소 빈 공간을 확보하고 인덱스를 이름으로 지정하여 데이터의 주솟값을 저장하는 등의 동작을 한다.  
(length프로퍼티의 개수만큼 빈 공간을 확보하고 각 공간에 인덱스로 이름을 지정하는것이 아니라 인덱스에 값을 지정할 때!!!)  
즉, 값이 지정되지 않은 인덱스는 '아직은 존재하지 않는 프로퍼티'에 지나지 않는다.

### null

비어있음을 명시적으로 나타내고 싶을 때 null을 써주면 된다.  
<span style="color: gray">(사용자가 '없음'을 표현하기 위해 명시적으로 undefined를 사용하는것은 지양하는 것이 좋다.)</span>  
*null의 주의할 점은 typeof null이 object라는 점이다.*  

```javascript
var n = null;
console.log(typeof n); // object

console.log(n == undefined); // true
console.log(n == null); // true
console.log(n === undefined); //false
console.log(n === null); // true
```

typeof null의 결과가 object이고 undefined와 동등연산자로 비교할 경우 같다고 판단한다..
따라서 어떤 값이 null인지 확인하는 방법으로 일치 연산자( === )를 사용해야만 정확히 판별할 수 있다.  
