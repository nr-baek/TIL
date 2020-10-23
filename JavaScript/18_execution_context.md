> [poiemaweb.com 참조](https://poiemaweb.com)

# 실행 컨텍스트

실행 컨텍스트(execution context)는 소스코드를 실행하는데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.  

## 소스코드의 타입

- 전역 코드
  - 전역에 존재하는 소스코드를 말한다.
  - 전역에 정의된 함수, 클래스 등의 내부 코드는 포함되지 않는다.
  - var 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수를 전역 객체의 프로퍼티와 메서드로 바인딩하고 참조하기 위해 전역 객체와 연결되어야 한다.
- 함수 코드
  - 함수 내부에 존재하는 소스코드를 말한다.
  - 함수 내부에 중첩된 함수, 클래스 등의 내부 코드는 포함되지 않는다.
  - 함수 코드는 지역 스코프를 생성하고 지역 변수, 매개변수, arguments 객체를 관리해야 한다.
- eval 코드
  - 빌트인 전역 함수인 eval함수에 인수로 전달되어 실행되는 소스코드를 말한다.
- 모듈 코드
  - 모듈 내부에 존재하는 소스코드를 말한다.
  - 모듈 내부의 함수, 클래스 등의 내부 코드는 포함되지 않는다.

소스코드는 그 타입에 따라 실행컨텍스트를 생성하는 과정과 관리 내용이 다르기 때문에 위의 내가지 타입으로 구분한다.  

## 소스코드의 평가와 실행

자바스크립트 엔진은 소스코드를 처리할 때 **평가**와 **실행** 2가지의 과정을 거친다.  

### 소스코드의 평가 과정

소스코드 평가 과정에서는 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문 만을 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록한다.  

### 소스코드의 실행 과정

소스코드의 평가 과정이 끝나면 선언문을 제외한 소스코드가 순차적으로 실행되기 시작한다. (선언문은 소스코드의 평가 과정에서 이미 실행되었기 때문)  
소스코드의 실행 과정이 런타임이다.  
이때 소스코드 실행에 필요한 정보, 즉 변수나 함수의 참조를 실행 컨텍스트가 관리하는 스코프에서 검색해서 취득한다.  
그리고 변수 값의 변경과 같은 소스코드의 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록된다.  

## 실행 컨텍스트의 역할

실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.  
좀 더 구체적으로 말해, 실행 컨텍스트는 식별자(변수, 함수, 클래스 등의 이름)를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 매커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.  
식별자와 스코프는 실행 컨텍스트의 렉시컬 환경으로 관리하고 코드 실행 순서는 실행 컨텍스트 스택으로 관리한다. 먼저 실행 컨텍스트 스택에 대해 살펴보자.

## 실행 컨텍스트 스택

자바스크립트 엔진은 실행 컨텍스트 스택으로 코드 실행 순서를 관리한다.  
먼저, 스택이란 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO(Last In First Out) 형식의 자료 구조이다.  
가장 나중에 들어온 데이터가 가장 먼저 나가는 방식이다.  
자바스크립트 엔진은 먼저 전역 코드를 평가하여 전역 실행 컨텍스트를 생성한다. 그리고 함수가 호출되면 함수 코드를 평가하여 함수 실행 컨텍스트를 생성한다.  
이떄 생성된 실행 컨텍스트는 스택 자료구조로 관리되는데 이를 **실행 컨텍스트 스택**(또는 콜 스택)이라고 부른다.  

```javascript
const x = 1;

function foo () {
  const y = 2;

  function bar () {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6


// 전역 실행 컨텍스트 > foo 함수 실행 컨텍스트 > bar 함수 실행 컨텍스트 순으로 실행 컨텍스트 스택에 각 실행 컨텍스트가 쌓이게(push) 된다.
// 그러다 bar 함수 실행 컨텍스트 > foo 함수 실행 컨텍스트 > 전역 실행 컨텍스트 순으로 실행 컨텍스트 스택에서 제거(pop)된다.
```

1. 전역 코드의 평가
    - 전역 코드 평가 및 전역 실행 컨텍스트 생성
    - 실행 컨텍스트 스택에 push(스택: 전역)
    - 전역 변수 x의 식별자와 전역 함수 foo가 실행 컨텍스트에 등록
2. 전역 코드 실행
    - 코드 실행 중 전역 변수x에 값이 할당되고 foo함수 호출
    - foo함수 호출과 동시에 전역 코드의 실행이 일시 중단되고 foo함수 내부로 이동
3. foo함수의 코드 평가
    - 함수 코드 평가 및 foo함수의 실행 컨텍스트 생성
    - 실행 컨텍스트 스택에 push(스택: 전역 - foo)
    - 지역변수 y의 식별자와 중첩함수 bar가 실행 컨텍스트에 등록된다.
4. foo함수의 코드 실행
    - 지역 변수 y에 값이 할당되고 중첩함수 bar 호출
    - bar함수 호출과 동시에 foo함수의 실행이 일시 중단되고 bar함수 내부로 이동
5. bar함수의 코드 평가
    - 함수 코드 평가 및 bar함수의 실행 컨텍스트 생성
    - 실행 컨텍스트 스택에 push(스택: 전역 - foo - bar)
    - 지역변수 z의 식별자가 실행 컨텍스트에 등록된다.
6. bar함수의 코드 실행
    - 지역변수 z에 값이 할당되고 console.log메서드를 호출
7. console.log메서드 호출
    - 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 push
    - (스택: 전역 - foo - bar - console.log)
    - 인수 값 출력 후 console.log메서드 종료되며 스택에서 pop
    - ((스택: 전역 - foo - bar)
8. bar 함수 코드로 복귀 및 bar 함수 종료
    - 실행 컨텍스트 스택에서 pop (스택: 전역 - foo)
9. foo 함수 코드로 복귀 및 foo 함수 종료
    - 실행 컨텍스트 스택에서 pop (스택: 전역)
10. 전역 함수 코드로 복귀 및 전역 함수 종료
    - 실행 컨텍스트 스택에 아무것도 남아있지 않음
  
**실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트다.**  

## 렉시컬 환경

렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트이다.  
렉시컬 환경은 **환경 레코드**, **외부 렉시컬 환경에 대한 참조** 이렇게 두 개의 컴포넌트로 구성된다.  

### 환경 레코드(Environment Record)

스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소다.  
환경 레코드는 소스 코드의 타입에 따라 관리하는 내용에 차이가 있다.  

### 외부 렉시컬 환경에 대한 참조(Outer Lexical Environment Reference)

외부 렉시컬 환경에 대한 참조는 상위 스코프를 가리킨다.  
상위 스코프란 외부 렉시컬 환경, 즉 **해당 실행 컨텍스트를 생성한 소스코드를 포함하는 상위 코드의 렉시컬 환경**을 말한다.  
외부 렉시컬 환경에 대한 참조를 통해 스코프체인을 구현한다.  

## 전역 코드 평가

전역 소스코드가 로드되면 자바스크립트 엔진은 전역 코드를 평가 한다.  

## 전역 코드 평가 순서

1. 전역 실행 컨텍스트 생성
2. 전역 렉시컬 환경 생성
    - 전역 환경 레코드 생성
        - 객체 환경 레코드 생성
        - 선언적 환경 레코드 생성
    - this 바인딩
    - 외부 렉시컬 환경에 대한 참조 결정
  
![전역 실행 컨텍스트와 렉시컬 환경에 대한 그림](https://images.velog.io/images/ursr0706/post/fe76ec0c-73c9-41b9-89a7-ac9fba7f6a15/23-9.png)

전역 실행 컨텍스트와 렉시컬 환경은 위와 같은 내용으로 구성되어있다.  

```javascript
var x = 1;
let y = 2;
```

전역 실행 컨텍스트의 내용을 토대로 위의 전역 코드를 평가하여 생성되는 전역 실행 컨텍스트의 구성을 객체로 표현해 보면 아래와 같다.

```javascript
cosnt GlobalExecutionContent = {
  GlobalLexicalEnvironment : {
    GlobalEnvironmentRecord: {
      ObjectEnvironmentRecord : {
        BindingObject: {
          window: { x: 1}
        }
      },
      DeclarativeEnvironmentRecord: { y:2},
      [[GlobalThisValue]]: window
    },
    OuterLexicalEnvironmentReference: null
  }
}
```

## 전역 환경 레코드

전역 환경 레코드는 객체 환경 레코드(Object Environment Record)와 선언적 환경 레코드(Declarative Environment Record)로 구성되어 있다.  

### 객체 환경 레코드(Object Environment Record)

객체 환경 레코드는 BindingObject라고 부르는 객체와 연결된다.  
전역 코드 평가 과정에서 var 키워드로 선언한 전역 변수와 함수 선언문으로 정의된 전역 함수는 전역 환경 레코드의 객체 환경 레코드에 연결된 BindingObject를 통해 전역 객체의 프로퍼티와 메서드가 된다. 그리고 이때 등록된 식별자를 전역 환경 레코드의 객체 환경 레코드에서 검색하면 전역 객체의 프로퍼티를 검색하여 반환한다.  

### 선언적 환경 레코드

var 키워드로 선언한 전역 변수와 함수 선언문으로 정의한 전역 함수 이외의 선언, 즉 let, const 키워드로 선언한 전역 변수(let, const 키워드로 선언한 변수에 할당한 함수 표현식 포함)는 선언적 환경 레코드에 등록되고 관리된다.  
(let, const 키워드로 선언한 변수도 변수 호이스팅이 발생하는 것은 변함이 없다. 단, let, const 키워드로 선언한 변수는 런타임에 컨트롤이 변수 선언문에 도달하기 전까지 일시적 사각지대에 빠지기 때문에 참조할 수 없다.)

### this 바인딩

전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에 this가 바인딩된다. 일반적으로 전역 코드에서 this는 전역 객체를 가리키므로 전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에는 전역 객체가 바인딩된다.  

## 외부 렉시컬 환경에 대한 참조 결정

외부 렉시컬 환경에 대한 참조(Outer Lexical Environment Reference)는 현재 평가 중인 소스코드를 포함하는 외부 소스코드의 렉시컬 환경, 즉 상위 스코프를 가리킨다. 이를 통해 단방향 링크드 리스트인 스코프 체인을 구현한다.  
전역 코드를 포함하는 소스코드는 없으므로 전역 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 null이 할당된다.  

## 실행 컨텍스트와 블록 레벨 스코프

let, const 키워드로 선언한 변수는 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정하는 블록 레벨 스코프(block-level scope)를 따른다.

```javascript
let x = 1;

if (true) {
  let x = 10;
  console.log(x); // 10
}

console.log(x); // 1
```

위의 코드에서 if 문의 코드 블록 내에서 let 키워드로 변수가 선언되었다. 따라서 if 문의 코드 블록이 실행되면 if 문의 코드 블록을 위한 블록 레벨 스코프를 생성해야 한다. 이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존의 전역 렉시컬 환경을 교체한다. 이때 새롭게 생성된 if 문의 코드 블록을 위한 렉시컬 환경의 외부 렉시컬 환경에 대한 참조는 if 문이 실행되기 이전의 전역 렉시컬 환경을 가리킨다.  
if 문 코드 블록의 실행이 종료되면 if 문의 코드 블록이 실행되기 이전의 렉시컬 환경으로 되돌린다.  
이런식으로 if문 외의 모든 블록문에 블록 레벨 스코프가 적용된다.