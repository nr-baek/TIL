# C89

- 1989년 ANCI(American National Standards Institute)에 의해 채택된 첫 번째 C 표준
  - ANSI C라고도 부름
- 30년이나 지났지만 아직도 대부분의 컴파일러가 지원하는 표준
- 오늘날 대부분의 C 코드도 이 버전
  - 많은 임베디드 시스템(C의 주 사용분야)은 여전히 C89만 지원
  - 특화된 소형 하드웨어는 그것을 직접 제어하는 전용 운영체제를 지원함
  - 즉, 윈도우 및 리눅스처럼 발전된 기능을 갖춘 범용 운영체제를 지원 안함
  - 그 하드웨어 + 운영체제 용으로 컴파일이 가능하면서도 하드웨어에 직접 접근할 수 있는 컴파일러를 제작해야 한다면 C89표준이 가장 간단

## Hello World 출력

```c
#include <stdio.h>

/* 프로그램의 시작점 */
int main(void)
{
    printf("Hello World\n");
    return 0;
}
```

- 실행해보면 `Hello World`가 출력이 됨

### #include

|C#|C|
|-|-|
|using System;|#include <stdio.h>|

- C#에서의 using 지시문(directive)과 비슷한 일을 함
- 다른 파일에 구현된 함수나 변수를 사용할 수 있게 해 줌
- 다만, C#처럼 똑똑하게 알아서 함수나 변수를 찾아주지는 않음
  - C#보다는 좀 무식한 방법으로 찾아옴
- #include는 전처리기 지시문 중 하나
  - C언어에서는 #으로 시작하는 문장을 _전처리기_ 라고 부름
- 전처리기(preprocessor)란?
  - 컴파일을 하기 전에 텍스트(소스코드 같은 것들)를 '복붙'해주는 일을 함
- `#include <stdio.h>`
  - < >안의 단어(예: stdio.h)는 실제 디스크 상에 존재하는 파일 이름
  - = 'stdio.h'파일 찾아서 '포함'해줘!
  - 컴파일러에게 이 소스코드를 번역하기 위해 stdio.h파일을 먼저 참조하라고 지시하는 것
- #include의 동작 방법
  1. 예를들어 hello.c파일을 컴파일 할 때 hello.c의 헤더 파일을(#include <stdio.h>) 발견!
  2. 컴파일을 시작하기 전 전처리기가 #include에 있는 <stdio.h>파일을 찾아서 열어봄
  3. <stdio.h>파일의 내용을 모두 복사함
  4. hello.c파일의 #include <stdio.h> 라고 쓰여 있는 부분을 복사한 내용으로 바꿈
  5. 결국 hello.c파일을 컴파일 하는 도중 stdio.h에 들어있는 다른 함수들도 같이 컴파일 함(주먹구구)

#### <stdio.h>

- C표준 라이브러리(C Standard aLibrary, libc)중 일부
  - C표준 라이브러리란?
    - 다음에 필요한 매크로, 자료형, 함수 등을 모아 놓은 것
      - 문자열 처리
      - 수학 계산
      - 입출력 처리
      - 메모리 관리
- <stdio.h>의 역할
  - libc에서 표준 입출력(Standard Input and Ountput)을 담당
  - 스트림 입출력에 관련된 함수들을 포함
  - C#의 System 네임스페이스와 비슷한 역할을 가진 라이브러리
  - stdio라이브러리에 있는 함수의 몇 가지 예:
    - printf( )
    - scanf( )
    - fopen( )
    - fclose( )

### main(void) 함수

```c
int main(void)
{
    return 0;
}
```

- 프로그램의 진입점(entry point)
- c코드를 빌드해서 나온 실행파일(.exe 또는 .out)을 실행하면 main(void)함수가 자동적으로 실행됨
- 프로그램의 시작을 나타내려면 main함수를 반드시 정의해야 함
  - main함수를 만들지 않거나 혹은 2개 이상 선언하면 링크할 때 오류남

#### main() 함수의 이름과 반환형

|C#|C|
|-|-|
|static void Main(string[ ]args)<br  />{<br  />}|int main(void)<br  />{<br  />return 0;<br  />}|

- C#의 Main() 함수와 마찬가지
  - 하지만, C는 대문자 'M'대신에 소문자'm'을 사용
  - 약속된 함수명이기 때문에 바꿀 수 없음
- 반드시 int를 반환해야 함
  - `return 0;`
    - 프로그램을 실행시킨 사람이 프로그램이 정상적으로 종료됐는지 판단할 때 쓰는 코드
    - 0: 프로그램에 문제가 없었다는 뜻
    - 0이 아닌 양수를 반환한다면 프로그램 실행중 어떤 문제가 있었다는 의미
- `(void)`
  - 다른 언어와 달리 void를 생략한다고 매개변수가 없다는 뜻이 아님
  - C에는 함수 선언과 정의가 있음
    - 함수 선언에서 void를 생략하면 매개변수를 받는다는 의미
      - 단,아직 매개변수의 갯수와 자료형을 모를 뿐
    - 함수 정의에서 void를 생략하면 매개변수가 없다는 뜻
  - 따라서, 언제나 void를 넣는 습관을 기르자

  ```c
  int sum(void); /* 함수 선언: 매개변수가 없음 */
  int sum();     /* 함수 선언: 매개변수가 있는데, 아직 뭔지 모름*/

  int sum(void) /* 함수 정의: 매개변수가 없음 */
  {
      /* 코드 생략 */
  }

  int sum()     /* 함수 정의: 매개변수가 없음 */
  {
      /* 코드 생략 */
  }
  ```

### printf( ) 함수

- 화면(콘솔 창)에 데이터를 출력할 때 사용하는 함수
- printf의 뜻은 'print formatted'(서식에 맞게 출력하다)

`printf("Hello World!\n");`

- "Hello World!"를 콘솔 창에 출력
- 그리고 줄을 바꿈 -> '\n'
- C# System.Console.Write 함수와 같음
  - C#은 '+'나 스트링포맷, 혹은 문자열 보간($"{}")을 이용
  - C는 그런 거 없음..
  - C는 '%s'라는 서식문자가 문자열이 들어갈 위치를 알려줌

  ```c
  const char* name = "BloodyMary";
  printf("Hello, %s\n", name);
  ```

### 주석(comment)

`/* entry point for the program */`

- `/* */`만 지원(최소한 c89에서는)
- 주석이 한 줄이든 여러 줄이든 다 /* */만
  - 다른 언어에선 여러 줄에는 /* */를 한 줄에는 //를 쓰는게 일반적
