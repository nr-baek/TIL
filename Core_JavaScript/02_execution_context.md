# 실행 컨텍스트

실행할 코드에 제공할 환경 정보들을 모아놓은 객체  
(= 함수를 실행할 때 필요한 환경정보를 담은 객체)  

## 실행 컨텍스트와 콜스택

동일한 환경에 있는 코드들을 실행할 때(=함수가 실행될 때) 필요한 환경 정보들을 모아 컨텍스트를 구성하고, 이를 콜스택에 쌓아올린다.  
*콜스택(call stack)이란 현재 어떤 함수가 동작하고 있는지, 다음에 어떤 함수가 호출돼야 하는지 등을 제어하는 자료구조이다.*  

```javascript
var a = 1;
function outer() {
  console.log(a); // 1

  function inner() {
    console.log(a);  // undefined
    var a = 3;
  }
  inner();
  console.log(a);  //1
}
outer();
console.log(a); //1
```

실행 컨텍스트가 콜스택에 담기고 제거되는 순서

1. **[ 비어있음 ]**
  자바스크립트 파일 실행 전
2. **[ 전역 컨텍스트 ]**
  자바스크립트 파일이 열리는 순간 전역 컨텍스트가 활성화 된다.
3. **[ 전역 컨텍스트 - outer ]**
  전역 컨텍스트와 관련된 코드를 순차로 진행하다가 outer함수를 호출.  
  outer에 대한 환경 정보를 수집해서 outer 실행 컨텍스트를 생성한 후 콜스택에 담는다.
  이 때 전역 컨텍스트와 관련된 코드의 실행을 일시중단하고 outer함수 내부의 코드들을 순차로 실행한다.
4. **[ 전역 컨텍스트 - outer - inner ]**
  마찬가지로 inner함수의 실행 컨텍스트가 콜스택의 가장 위에 담기면 outer컨텍스트와 관련된 코드의 실행을 중단하고 inner함수 내부의 코드를 순서대로 진행한다.
5. **[ 전역 컨텍스트 - outer ]**
  inner함수의 실행이 종료되면 inner실행 컨텍스트가 콜스택에서 제거된다.  
  그리고 나서 outer실행 컨텍스트의 중단됐던 지점부터 이어서 실행한다.
6. **[ 전역 컨텍스트 ]**
  outer실행 컨텍스트도 실행이 종료되면 콜스택에서 제거되고 전역 컨텍스트만 남게 된다.
7. **[ 비어있음 ]**
  전역공간에도 더는 실행할 코드가 남아있지 않으면 전역 컨텍스트도 콜스택에서 제거되고, 콜스택에는 아무것도 남지 않은 상태로 종료된다.

## 실행 컨텍스트의 수집 정보

- **VariableEnvironment**: 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보
  - 선언 시점의 LexicalEnvironment의 스냅샷
  - 변경사항은 반영되지 않음

- **LexicalEnvironment**: 처음엔 VariableEnvironment와  같지만 변경 사항이 실시간으로 반영됨.

- **ThisBinding**: this 식별자가 바라봐야 할 대상 객체.

어떤 실행 컨텍스트가 활성화될 때 자바스크립트 엔진은 해당 컨텍스트에 관련된 코드들을 실행하는 데 필요한 환경 정보(VariableEnvironment, LexicalEnvironment, ThisBinding)들을 수집해서 실행 컨텍스트 객체에 저장한다.  

### VariableEnvironment

VariableEnvironment에 담기는 내용은 LexicalEnvironment와 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다르다.
environmentRecord와 outerEnvironmentReference로 구성되어있다.

### LexicalEnvironment

'사전적인 환경'으로 표현할 수 있는데, 현재 컨텍스트 내부의 식별자들(environmentRecord)과 그 외부 정보를 참조(outerEnvironmentReference)하도록 구성돼 있다.  

#### environmentRecord

environmentRecord에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다.  

컨텍스트를 구성하는 함수에 지정된 매개변수 식별자,  
선언한 함수가 있을 경우 그 함수 전체,  
var로 선언된 변수의 식별자 등이 식별자에 해당한다.  
(컨텍스트 내부 전체를 처음부터 끝까지 쭉 훑어나가며 **순서대로** 수집한다.)  

실행 컨텍스트의 코드가 실행되기 전 자바스크립트 엔진은 이미 해당 환경에 속한 코드의 변수명들을 모두 알고 있는 셈이다.  
(마치 식별자들을 최상단으로 끌어올려놓은 다음 실제 코드를 실행하듯)

##### 호이스팅(hoisting)

변수 정보를 수집하는 과정의 쉬운 이해를 위한 가상의 개념.  
실제하는 현상은 아니고 이해를 위해 만들어진 추상적 개념이다.  
식별자 정보를 실행 컨텍스트의 맨 위로 끌어올리는것이 호이스팅의 규칙이다.

```javascript
// 호이스팅 전

console.log(a());
console.log(b());
console.log(c());

function a() {
  return 'a';
}

var b = function bb() {
  return 'bb';
}

var c = function () {
  return 'c';
}
```

실행 컨텍스트가 처음 생성이 되는 순간 enviromentRecord에 정보를 수집함.  
위의 코드를 예시로 보면 enviromentRecord에

```javascript
{
  function a(){...},
  b,
  c
}
```

이와같은 정보의 객체가 저장되었다고 볼 수 있다.
호이스팅 처리를 한 후에는 아래와 같은 코드가 된다.

```javascript
// 호이스팅을 마친 상태

function a() {  // 함수 선언은 전체를 끌어올린다.
  return 'a';  
}
var b; // 변수는 선언부만 끌어올린다.
var c;

console.log(a());
console.log(b());
console.log(c());

b = function b() {
  return 'bb';
}
c = function c() {
  return 'c';
}
```

위의 호이스팅 처리를 보면 변수 함수 선언문과 함수 표현식에 대한 호이스팅 처리가 다른 것을 알 수 있다.  
함수 선언문은 전체를 호이스팅한 반면 함수 표현식은 변수 선언부만 호이스팅한다.  
때문에 함수 선언문은 함수가 어느 위치에 선언되었는지에 상관 없이 어디에서나 함수를 사용할 수 있는 반면,  
함수 표현식은 함수를 선언한 다음위치에서만 정상적으로 동작한다.  
때문에 상대적으로 함수 표현식이 안전하다.  

#### outerEnvironmentReference

외부 환경에 대한 참조(현재 문맥에 관련있는 외부 식별자 정보를 수집한 것)가 outerEnvironmentReference이다.
바로 직전 컨텍스트의 LexicalEnvironment를 참조한다.

##### 스코프(scope)

**스코프**란 식별자에 대한 유효범위이다.  
함수에의해 생성된 스코프의 밖에서 선언한 변수는 스코프 외부뿐만 아니라 내부에서도 접근 가능하지만,  
스코프 안에서 선언한 변수는 오직 스코프 내부에서만 접근할 수 있다.  
식별자의 유효범위는 '실행 컨텍스트'가 만들어내는데, 실행 컨텍스트가 수집해놓은 정보에만 접근할 수 있기 때문이다.
이러한 '식별자의 유효범위'를 안에서부터 바깥으로 차례로 검색해 나가는 것을 **스코프 체인**이라 하며, 이를 가능케 하는 것이 바로 outerEnvironmentReference이다.

##### 스코프 체인(scope chain)

```javascript
var a = 1;
function outer() {
  console.log(a); // 1

  function inner() {
    console.log(a);  // undefined
    var a = 3;
  }
  inner();
  console.log(a);  //1
}
outer();
console.log(a); //1
```

|콜스택 | 실행 컨텍스트 수집 정보 |
|:-:|-|
|inner|**LexicalEnvironment**</br>environmentRecord</br>outerEnvironmentReference|
|outer|**LexicalEnvironment**</br>environmentRecord</br>outerEnvironmentReference|
|전역|**LexicalEnvironment**</br>environmentRecord|

inner 컨텍스트의 outerEnvironmentReference는 outer의 LexicalEnvironment를 참조한다.  
outer 컨텍스트의 outerEnvironmentReference는 전역 컨텍스트의 LexicalEnvironment를 참조한다.  
이렇게 **스코프체인**현상은 outerEnvironmentReference가 만드는 것이다.  

inner컨텍스트에서 environmentRecord를 통해 자신의 컨텍스트에서 생성된 변수에 접근을 할 수 있고 outerEnvironmentReference가 참조하는 outer컨텍스트의 LexicalEnvironment를 통해 outer컨텍스트에서 생성된 변수에도 접근 할 수 있다. 또한 outer컨텍스트의 outerEnvironmentReference를 통해서 전역 컨텍스트의 변수에도 접근할 수 있다.  

또한 각 outerEnvironmentReference는 오직 자신이 선언된 시점의 LexicalEnvironment만 참조하고 있으므로 가장 가까운 요소부터 차례대로만 접근할 수 있고 다른 순서로 접근하는 것은 불가능하다.  
이런 구조적 특징으로 여러 스코프에서 동일한 식별자를 선언한 경우에는 **무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능**하게 된다.  

## 지역변수와 전역변수

지역변수: 함수 내부에서 선언한 변수  
전역변수: 전역 공간에서 선언한 변수  

*코드의 안전성을 위해 가급적 전역변수 사용을 최소화하도록 하는것이 좋다.*