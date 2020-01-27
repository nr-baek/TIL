# switch문

## 예) 간단한 메뉴판

```c
Console.WriteLine("Please select a menu");
Console.WriteLine("1. Cheese burger");
Console.WriteLine("2. Double cheese burger");
Console.WriteLine("3. Veggie burger");
Console.WriteLine("4. Cheese & mushroom burger");

int menu = int.Parse(Console.ReadLine());

switch(menu)
{
    case 1:
        Console.WriteLine("You selected a cheese burger");
        Console.WirteLine("Price: $3");
        break;
    case 2:
        Console.WriteLine("You selected a double cheese burger");
        Console.WriteLine("Price: $4.5");
        break;
    case 3:
        Console.WriteLine("You selected a veggie burger");
        Console.WriteLine("Price: $4");
        break;
    case 4:
        Console.WriteLine("You selected a mushroom burger");
        Console.WriteLine("Price: $5");
        break;
    default:
        Console.WriteLine("Please enter a correct number(1~4)");
        break;
}
```

- 매치 표현식의 결과 값에 따라 실행할 구문을 선택
  - 경우에 따라 if/else문보다 switch문의 가독성이 더 좋을때가 있음.
    - mathScore > 90 이런 조건 말고
    - num == 1, num == 2, num == 3 이렇게 패턴 일치가 되는 조건들
- if/else if문은 여러 조건의 내용이 있을 수 있지만 switch문은 하나의 조건으로만 검사한다는 것이 확실하기 때문에 읽기도 쉽고 실수를 줄일 수 있다.
- 특정한 조건에서는 switch문을 써주는 것이 좋다.

|if/else문이 더 좋은 경우|switch문이 더 좋은 경우|
|------|-----|
|수학 성적이 100점 일 때<br  />수학 성적이 90점 이상 100점 미만일 때<br  />수학 성적이 80점 이상 90점 미만일 때|성적이 A일 때<br  />성적이 B일 때<br  />성적이 C일 때|
|>, >=, <=, !=|==|

```c
switch(매치 표현식)
{
    case <상수 라벨1>:
        매치 표현식의 반환값이 상수 라벨1과 일치할 때 실행하는 구문;
        break;
    case <상수 라벨2>:
        매치 표현식의 반환값이 상수 라벨2과 일치할 때 실행하는 구문;
        break;
    case <상수 라벨n>:
        매치 표현식의 반환값이 상수 라벨n과 일치할 때 실행하는 구문;
        break;
    default:
        매치 표현식의 반환값이 위의 상수 라벨과 하나도 일치하지 않을 때 실행하는 구문;
        break;
}
```

### default 구문

- 매치 표현식의 반환값과 일치하는 case구문이 없을 경우 실행
- 때로는 이렇게 잘못되거나 예상치 못한 반환값을 잡아야 할 때가 있음
  - 이 경우 어서트(sssert)를 사용함
- default구문의 끝에도 반드시 break구문을 넣을 것.

### case에서 사용할 수 있는 상수형

- int[기본]
- long
- char
  - char 역시 정수형이기 때문!
- bool (bool값을 넣을 때는 switch문 보다는 if/else문을 많이 씀)
- string(C# 전용_string은 원래 기본 자료형이 아님)
- 부동소수점형들은 사용 불가

### case 여러개를 묶을 수도 있음

```c
case 1:
    실행구문;
    break;
case 2:
case 3:
    실행구문;
    break;
default:
    실행구문;
    break;
```

- case 2와 3 일때 실행구문을 같이 할 수 있음
- case 2를 선택하면 fall through로 case 3의 실행구문이 실행 됨
