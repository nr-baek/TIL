# Java

## 설치

검색창에 *jdk(java development kit) download*검색 후 운영체제에 맞는 파일을 다운받아 설치한다.  
(설치 후 terminal창에 java -version 그리고 javac -version을 입력했을 때 에러없이 나오면 설치 잘된 것)

## 개발환경 셋팅하기 - eclipse

1. 검색창에 'eclipse'를 검색해서 다운받는다.
2. eclipse installer를 클릭해서 실행한다.
3. Eclipse IDE for Java Developers 선택 및 install
4. 약관동의, 설치완료 후 Launch

## Java 애플리케이션 실행

1. eclipse를 실행
    * 실행 후 나타나는 화면 중 Task List 와 Outline은 끄고(나중에 필요할때 켠다.) 왼쪽/ 중앙/ 아래쪽 세 공간만 남긴다.
    * 프로젝트 관리화면인 왼쪽의 Package Explorer는 파일들을 프로젝트관리에 편리하게 바꿔서 보여주기 때문에 있는 그대로를 보여주는 Navigator를 대신 띄운다.(Navigator가 입문자에게 더 적합하다고함)
2. 폴더 만들기  
    Create a Java project 클릭 -> Use Defalut location 체크해제 후 Browse버튼으로 경로 선택 -> project name입력 -> Finish
3. 프로젝트폴더에 파일생성
    마우스 우클릭 -> New -> File(File name: HelloWorldApp.java)->내용작성

    "Hello World!!"문자가 출력되게 만들기

    ```
    public class HelloWorldApp{
       public static void main(string[] args){
           System.out.println("Hello World!!");
       }
    }
    ```

    java파일을 작성하고 저장을 하면 같은 파일명을 갖은 class확장자의 파일이 생성된다.  
    HelloWorldApp.java를 실행하라고 명령을 하면 java는 파일이름과 똑같은 class인 HelloWorldApp.class를 찾고 main메소드 중괄호{}안의 코드를 실행한다.
    _프로그램을 실행시킨다는 것은 class를 실행시키는 것과 비슷한 맥락._
    HelloWorldApp.java  -> 사람이 읽고 쓸 수 있는 소스코드  
    HelloWorldApp.class -> java파일을 저장했을 때 Java가 **compile**이라는 과정을 거쳐 컴퓨터가 읽을 수 있는 파일인 확장자가 class인 파일을 만든다.

4. 파일실행
마우스우클릭 -> Run As -> Java Application -> 파일이 실행되면서 console창에 "Hello World!!"가 출력된다.

## Java 동작원리

```
 [원인]                          [결과]
Source(원천)                    Application
Code(부호)              ->      Program
Language(약속된_언어)
```

**Java Source Code(.java)** -> _Compile_ -> **Java Application(.class)** -> _Run_ -> **Java Virtual Machine(자바가상머신)** -> _Run_ -> **Computer**

## 데이터와 연산

 숫자(Number)
 문자(String)

```
public class Datatype{
    public static viod main(string[] args){
        System.out.println(6);    // number
        System.out.println("six");   // string
        System.out.println("6");   // string
        System.out.println(6+6);   // 12 number
        System.out.println("6"+"6");   // 66 string
        System.out.println(6 * 6);   // 36 number
        System.out.println("6"*"6");   // 오류 
        System.out.println("3333".length());  // 4 string
        System.out.println(3333.length());   // 오류
    }
}
// 숫자를 표현할때는 그냥 숫자를 입력하면 되지만 문자열을 표현할 때는 큰따옴표("")로 묶어준다.
// '+ - / *' 는 숫자의 연산
// length()는 문자의 글자 수를 세는 연산
```

_프로그래밍에서는 데이터 타입을 구분해야하며 데이터 타입에 맞는 연산방법을 사용해야 한다. 데이터 타입에 맞지 않는 연산방법은 오류가 난다._
_어떤 데이터 타입이 존재하는지, 각 데이터 타입에는 어떤 연산이 존재하는지를 알면 컴퓨터에서 할 수 있는 일이 많아진다._

### 문자열의 표현

* 문자열은 "Hello World" 처럼 " " 큰따옴표 안에 입력으로 표현한다.  
    ('Hello World'처럼 작은따옴표를 사용하면 오류가 난다. )
* ' ' 작은따옴표는 character(문자)를 나타내는 데이터 타입으로 'H'와 같이 한 문자를 나타내는 데이터타입이다.
* "Hello World" 안에서 줄바꿈을 하려면 "Hello \nworld"와 같이 줄바꿈하려는 글자 앞에 **\n**(역슬래쉬+"n")을 입력해준다.
* 글자수를 출력하려면 "Hello World".length -> 11
* 다른 텍스트로 교체하고 싶을 때 "Hello World".replace("World","Coco") -> Hello Coco

## 변수

변수(값이 변할 수 있는 문자): 데이터에 붙이는 이름.

### 변수 지정하기

a=1; -> 오류.  
변수의 값에 어떤 데이터 타입이 들어갈 수 있는지 지정해줘야 한다.
```
String c="Hello World"; // 문자열의 데이터타입인 String을 지정해준다.  
int a=1;   // 정수(integer)인 데이터타입 int를 지정해준다.
double b=1.1;  //실수(realnumber)인 데이터타입 double을 지정해준다.
```

_이렇게 Java에서 변수를 만들때는 그 변수가 어떤 데이터 타입을 담을지 명확하게 해줘야 한다._
_(변수값의 데이터타입을 확실히 함으로써 나중에 그 변수를 꺼내쓸 때 데이터타입의 확신과 그에 따른 편리함 때문)_

### 변수의 효용

```
String name = nara
System.out.println("hi, "+name+"..."+name)

출력
hi, nara...nara
```

* 출력되는 내용에서 중복되는 문자를 수정해야 될 때 그 문자가 몇군데에 얼마나 있든 변수의 값만 바꾸면 되는 폭발적 효과
* 변수의 이름을 통해 코드를 보는사람으로 하여금 어떤 정보가 들어가 있는지 알 수 있게 함
    (코드를 짤 때 나도 타인도 코드를 봤을 때 그 의미를 빨리 파악할 수 있도록 짜는것이 중요하며 그 수단 중 하나가 '변수')

### 데이터 타입의 변환

* 정수형을 실수형으로, 실수형을 정수형으로 캐스팅
  * int c =1.1; --> 오류가 나면서 출력x  
    int c =(int)1.1  
    1.1은 double이지만 int의 형태로 바꾸는 코드(int)를 강제로 추가  
    (결과) 1  (0.1을 버리게 된다.)  

  * double b =1;  --> 오류없이 출력  
    (결과) 1.0 (버리는 값이 없음)  
    double b =1; 은 double b =(double) 1; 에서 (double)이 생략된 것이라고 보면 된다.

* 숫자 1을 문자열(String)으로 캐스팅
  * string b = Integer.toString(1);
  (결과) 1 (문자열 1로 출력)

## Programming

프로그램 만들기 - IoT 라이브러리 설치하기

1. 깃헙(github.com/egoin/java-iot)에서 zip파일을 다운받는다.
2. 다운받은 파일의 압축을 풀고 내 프로젝트파일에 붙여넣는다
3. Programming 프로젝트에 org파일을 붙여넣은 뒤에 class를 만든다.
4. 생각하기
    * 무엇을 할것인지, 그 것을 하기위해 시간의 순서에 따라 어떤일이 일어나야 하는지 생각해본다.
    elevator call -> security off -> light 0n ...
5. OkJavaGoInHome이라는 프로그램을 만들어 본다.

```
import org.opentutorials.iot.Elevator;
import org.opentutorials.iot.Lighting;
import org.opentutorials.iot.Security;
 
public class OkJavaGoInHome {
 
    public static void main(String[] args) {
         
        String id = "JAVA APT 507";
         
        // Elevator call 
        Elevator myElevator = new Elevator(id);
        myElevator.callForUp(1);
         
        // Security off 
        Security mySecurity = new Security(id);
        mySecurity.off();
         
        // Light on
        Lighting hallLamp = new Lighting(id+" / Hall Lamp");
        hallLamp.on();
         
        Lighting floorLamp = new Lighting(id+" / floorLamp");
        floorLamp.on();
 
    }
 
}

(출력)
JAVA APT 507 -> Elevator callForUp stopFloor : 1
JAVA APT 507 -> Security off
JAVA APT 507 / Hall Lamp -> Lighting on
JAVA APT 507 / floorLamp -> Lighting on

엘리베이터가 1층으로 오고, 시큐리티 해제, 불이켜지는 것 처럼 순서대로 실행이 됨.
```

### 디버거

bug : 입력한 코드에 의도하지 않게 생긴 문제  
debugging : bug를 잡는 것  
debugger : debugging 도구  

_이클립스 기반의 디버거_
입력한 코드중에 어떤 문제가 있거나 분석하고 싶을 때

1. 줄 번호쪽에 더블클릭으로 braek point를 지정해서 프로그램이 실행되는 것을 멈출 지점을 정한다.
2. 초록벌레모양(디버거버튼) 아이콘을 눌러서 실행한다.
    * 빨간네모버튼 : 디버깅 종료
    * Resume : break point부터 디버깅 시작
    * Step Over : 다음 줄 실행

### 입력과 출력

앞서 만든 OkJavaGoInHome 프로그램에서 실행시 팝업창이 뜨면서 사용자가 text정보를 입력하여 id값을 셋팅하도록 구현해보기.

1. 관련 코드 검색 'java popup input text swing'
(결과) JOptionPane.showInputDialog("Enter a path")
2. 작성해 둔 코드의 id변수의 값 부분을 'JOptionPane.showInputDialog("Enter a ID")'로 대체한다.
(JOptionPane은 없는 기능이기 때문에 읽어와야한다. _마우스를 올려서 'Import JOptionPane'을 선택해 클릭하면 import javax.swing.JOptionPane; 이라는 코드가 윗줄에 추가되면서 JOptionPane클래스가 로드 된다.)
3. 실행해보면 Input 팝업창이 뜨면서 "Enter a ID"메세지와 함께 입력란이 표시된다.
(이렇게 showInputDialog기능을 통해 팝업창에 사용자가 입력하는 값에 따라서 다르게 동작하는 프로그램을 만들 수 있다.)

```
import javax.swing.JOptionPane;
 
import org.opentutorials.iot.DimmingLights;
import org.opentutorials.iot.Elevator;
import org.opentutorials.iot.Lighting;
import org.opentutorials.iot.Security;
 
public class Ok_JavaGoInHomeInput {
 
    public static void main(String[] args) {
         
        String id = JOptionPane.showInputDialog("Enter a ID");
        String bright = JOptionPane.showInputDialog("Enter a Bright level");
         
        // Elevator call 
        Elevator myElevator = new Elevator(id);
        myElevator.callForUp(1);
         
        // Security off 
        Security mySecurity = new Security(id);
        mySecurity.off();
         
        // Light on
        Lighting hallLamp = new Lighting(id+" / Hall Lamp");
        hallLamp.on();
         
        Lighting floorLamp = new Lighting(id+" / floorLamp");
        floorLamp.on();
         
        DimmingLights moodLamp = new DimmingLights(id+" moodLamp");
        moodLamp.setBright(Double.parseDouble(bright));
        moodLamp.on();
 
    }
 
}
```

### Arguments(인자) & Parameter(매개변수)

Input값을 미리 정의해놓을 수 있다.

* Run Configurations > 실행파일 선택 > Arguments에 input값 입력
(' '로 묶어줘야 하나의 덩어리로 들어가며 띄어쓰기로 구분한다. ex_'Java APT' '33.0')

* 실행했을 때 정의해놓은 입력값을 받는 방법
  (프로그램에 매개변수를 통해서 인자를 전달하는 방식)
  이전에 입력한 값이 args변수에 담겨있는데 첫번째 값은 args[0], 두번째 값은 args[1]로 입력값을 받아올 수 있다. (프로그램은 0부터 카운트하는 습관이 있다.)

* 여러 형식의 Arguments를 입력해서 프로그램을 저장해서 원하는 형식의 프로그램을 실행하면 셋팅해둔 인자가 실행된다.

* Organize Run Favorites를 통해 자주 사용하는 프로그램을 목록에 추가할 수 있다.

## 직접 Compile하고 실행하기

다음과 같은 program.java파일을 만든다.

```
public class program{
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        System.out.println(3);
    }
}
```

Terminal 실행

1. 소스코드가 위치한곳으로 이동  
cd /Users/eclipse-workspace/programming

2. 명령어 ls를 입력해서 programming폴더의 내용을 확인해본다.(program.java파일이 있는지 확인)
3. 명령어 'javac program.java'를 입력해서 컴파일한다.(ls명령어로 program.class파일이 생성됐는지 확인)
4. program.java파일을 실행하기 위해 명령어 'java program' 를 입력한다.(java program.class라고 쓰지 않는다.)
5. 1 2 3이 출력된다.

_명령어로 java program을 입력하면 program.java와 같은 파일명의 program.class파일을 찾아들어가서 실행한다._
_class파일의 main메소드를 찾아 중괄호 안의 코드를 순차적으로 실행하여 출력한다._

### 라이브러리 이용

라이브러리: 다른사람이 사용할 수 있게 잘 정리정돈된 프로그램들  
미리 만들어져있는 부품을 로딩해서 컴파일+실행 하기  

* 이전에 만든 OkJavaGoInHome파일로 라이브러리를 이용한 파일을 컴파일 해보기
    (OkJavaGoInHome파일은 org라는 패키지를 다운받아서 Elevator, Lighting, Security기능을 import해서 사용했다.)

* javac OkJavaGoInHome.java 명령어로 컴파일하면 org폴더 안에있는 java파일도 같이 컴파일 되어 class확장자 파일이 생성된다.
    (org폴더의 class파일들을 지우고 javac OkJavaGoInHome.java를 명령해보면 class파일이 생성되는것을 확인할 수 있다.)

* 만약 org폴더의 위치가 변경된다면(org파일의 위치를 lib이라는 새로운 폴더안으로 옮김) OkJavaGoInHome파일과 같은 위치에 있지 않기 때문에 컴파일 되지 않는다.

* class path를 지정해서 org파일의 위치를 알려주면 컴파일 할 수 있다.  
    javac -cp ".:lib" OkJavaGoInHome.java (-cp ".:lib" ->컴파일할 소스를 현재위치와 lib폴더에서 찾아보라는 것)

* 실행할 때도 마찬가지로 org패키지가 같은 위치에 있지 않기 때문에 class path를 지정한다.
    java -cp ".:lib" OkJavaGoInHome

### 직접 컴파일해서 실행 할 때 입력값을 줘서 실행하기

OkJavaGoInHome에는 id변수의 값이 String id = args[0], String bright = args[1]과 같이 설정되어있다.  
CLI명령어창에서 OkJavaGoInHome프로그램을 실행하면 오류가 나기 때문에 args[0], args[1]에 해당되는 값을 입력해줘야한다. 
```
java OkJavaGoInHome "Java APT 507" "15.0"


args[0] -> Java APT 507
args[1] -> 15.0
```

