# overflow property와 그 쓰임새

>overflow는 요소의 콘텐츠가 너무 커서 요소의 **블록 서식 맥락에 맞출 수 없을 때**의 처리법을 지정하는 CSS속성이다.  

![자식높이에 따른 부모요소의 높이](https://images.velog.io/images/ursr0706/post/4a7abc46-5817-46cd-bd0e-d79206221db5/%EC%BA%A1%EC%B2%981.PNG)

부모요소의 높이는 자식요소의 높이만큼 설정된다.  
(부모요소의`height: auto;`이면 자식요소 또는 담고있는 텍스트의 높이만큼 설정된다.)  
이 때, 임의로 부모요소에 높이값을 지정한다면.  
그리고 그 높이가 자식요소의 높이보다 짧다면 어떻게 될까.  

![넘친 자식요소](https://images.velog.io/images/ursr0706/post/c1b55c18-2d9a-4d49-b623-4185f6d2c894/%EC%BA%A1%EC%B2%982.PNG)

부모요소의 높이는 작아졌지만 자식은 원래 갖고있던 높이를 유지하며 넘쳐 흐르는 현상이 나타난다.  
즉, 자식요소가 부모요소의 범위를 벗어나게 된다.  
이것을 제어하기 위해 나온 속성이 overflow이다.  

## overflow의 속성값  

### visible

overflow속성의 기본값이다.  
위의 예시에서 보이듯이 범위에서 넘쳐난 부분을 자르지않고 보이게한다.  

### hidden

![overflow_hidden](https://images.velog.io/images/ursr0706/post/1d0cc964-7591-4296-a2ae-0eea74b4d6c4/%EC%BA%A1%EC%B2%983.PNG)

콘텐츠를 부모요소에 맞게 잘라버려서 범위에서 넘친 부분이 보이지 않는다.  

### scroll

![overflow_scroll](https://images.velog.io/images/ursr0706/post/a5d53256-7f9a-4290-a7e7-edbf006cbabf/%EC%BA%A1%EC%B2%984.PNG)

콘텐츠를 부모요소의 범위에 맞게 잘라낸다.  
스크롤바를 이용해서 잘린 영역을 볼 수 있다.  
그러나 부모요소의 범위보다 큰지 작은지에 상관없이 가로, 세로의 스크롤바를 항상 표시한다.  
위의 예시에서도 보이듯 가로너비가 범위보다 크지 않은데도 스크롤바가 표시된 것을 볼 수 있다.

### auto

![overflow_auto](https://images.velog.io/images/ursr0706/post/b0fbbd9c-691b-4c79-a367-754e6ed3afa0/%EC%BA%A1%EC%B2%985.PNG)

범위의 넘침여부에 따라 스크롤바가 생긴다.  
만약 부모요소의 높이와 너비값 범위안에 있다면 visible값과 동일하게 보인다.

## float해제에 쓰이는 overflow속성

### float속성 사용시의 문제

float은 원래 한 요소가 컨테이너의 좌측이나 우측에 배치되게 하여 텍스트 및 inline요소가 그 주위를 감싸게 하기위해 지정하는 속성이다.  
이 float의 요소를 단순히 배치를 위한 목적으로만 쓰기도 한다.  
문제는 요소를 담은 부모인 컨텐이너의 높이값이 auto인 상황에서 안에 들어있는 text나 인라인요소가 충분치 않을 때, 혹은 안의 요소들이 모두 float상태일때 container가 높이값을 갖지 못하는 상황이 발생한다.  

![float적용 전](https://images.velog.io/images/ursr0706/post/fb26cd62-2083-4b9a-b93f-781d81fc246e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-20%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.00.39.png)

먼저 BOX1과 BOX2의 float적용 전의 모습이다.  
containerd의 height값은 auto로 되어있어서 box1과 box2가 놓여있는만큼의 높이값을 갖는다.  

![float적용 후](https://images.velog.io/images/ursr0706/post/e516f1b6-d508-473d-910a-5855bc7e7b25/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-20%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.01.32.png)

```css
.box1 {
float: left;
}
.box2 {
float: right;
}
```

BOX1은 왼쪽으로 배치되게, BOX2는 오른쪽으로 배치되게끔 하기 위해 각각 float속성을 이용하여 배치해보았다.  
그런데 container의 흰색 배경이 보여지지 않고 높이값 없는 black 테두리만 남겨진걸 볼 수 있다.  
block요소의 높이에 auto값이 있으면 자식요소의 높이만큼 높이값을 갖게 되는데 자식요소가 모두 normal flow에서 벗어났기 때문에 높이가 0이 되어버린 것이다.
이렇게 본래의 목적이 아닌 단순히 배치의 목적으로 쓰일때 나타나는 부작용을 해결하는 방법에 overflow속성이 쓰인다.

### overflow:hidden;을 사용한 float문제 해결

```css
.container {
overflow: hidden;
}
```

![float된요소 부모에 overflow](https://images.velog.io/images/ursr0706/post/99ad3b23-8e4d-46fb-9e8a-32e8eb59bb3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-20%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.22.03.png)

부모요소인 container에 `overflow:hidden;`속성을 넣어주었더니 높이가 0이였던 container의 높이값이 다시 자식요소인 box의 크기만큼 생겨났다.  
참 연관없는 속성일 것 같은데 어째서 overflow속성이 float의 문제를 해결한것일까?  

## overflow가 float의 문제를 해결한 원리

먼저, 부모요소는 float속성을 갖은 자식요소의 높이를 알지 못한다.  
그러나 body태그의 경우는 다르다. body태그는 float된 자식요소라 할지라도 그 높이값을 읽어낸다. body영역만큼은 다른 block요소와는 다르게 float된 자식요소의 높이를 알아내는데 전체를 다 표현한다.  
화면의 크기를 줄여도 scroll을 통해 자식요소를 모두 보여준다.  
body는 최상위이기 때문에 전체 요소를 표현해야하는 책임이 있기 때문이다.  

### 해결을 위한 접근

float할 요소를 품은 부모요소를 body태그처럼 만들면 되지 않을까?  
float가 적용된 요소의 높이까지도 읽어내는 body태그의 그 특성을 이용하는 것이다.  
그 특성을 가진 속성이 바로 **overflow**속성이다.  
부모요소에 overflow속성을 적용하면 그 부모요소는 작은 body가 된다.  

![overflowScroll](https://images.velog.io/images/ursr0706/post/a7c5bcc6-1bb2-4be1-97c7-0a1e179029f6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-20%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.47.11.png)

위의 그림은 BOX1과 BOX2에 각각 float속성을 넣은 것이다.  
그리고 부모요소인 container의 height를 임의로 box높이의 반만큼으로 지정하고나서 `overflow:scroll;`을 적용했다.
원래대로라면 Box1과 Box2는 container의 범위에서 벗어나서 넘치게 보였어야 하지만 `overflow:scroll;`속성을 적용했기때문에 scroll바가 생성되었고 scroll을 내려서 자식요소를 container안에서 온전히 읽을 수 있게 되어있다.  
마치 화면크기를 줄여도 온전히 모든 콘텐츠를 보여주는 body태그같다.  
body태그가 갖고있는 그러한 특성을 container에 적용하여 float문제를 해결한 것이다.  

### body태그가 가진 BFC(Block Formatting Context)의 특성

Block Formatting Context는 block요소가 그려질 수 있는 환경적인 부분을 담당하는 내부로직이다.  
body태그는 최상위 요소로서 Block Formatting Context이다.  
그런데 어떠한 요소에 `overflow`속성을 넣으면 새로운 Block Formatting Context를 만들어 넣은것과 같다.  
**`overflow`속성을 갖은 요소는 새로운 독립적인 문서가 시작된다.**  

![overflow_BFC](https://images.velog.io/images/ursr0706/post/5dc0f0f1-9d94-494f-8bba-a65a62bf31a3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.00.53.png)

새로운 Block Formatting Context가 되면서 별도의 레이어를 갖게 됐다고 봐도 된다.  
완전히 독립이 되어 별개의 영역이므로 float을 clear하지 않아도 `overflow`속성을 갖은 부모요소는 자식요소가 float되어도 높이값을 갖을 수 있으며, 다른 영역에 있는 요소와 겹치지 않게 된다.  
따라서 float를 사용할 때 부모요소에 `overflow`속성을 줌으로써 전혀 다른 영역으로 구분되게 해주기 때문에 유용하게 쓸 수 있다.

## 마진 병합 현상 해결에 쓰이는 overflow속성

[이전 글 - 마진 병합 현상에 대해 알아보기](https://github.com/nr-baek/TIL/blob/master/HtmlAndCss/Margin_Collapsing.md)

![H모양의 결과](https://images.velog.io/images/ursr0706/post/4b61a8cd-b155-45a5-b19a-735bd87f6702/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.09.20.png)

![부모자식의 마진 병합그림](https://images.velog.io/images/ursr0706/post/e9ee3ec2-be38-4353-a122-e97ed9684713/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.52.25.png)  

이렇게 자식요소인 A,B박스에 각각 50px의 마진을 줬을 때 부모자식간의 마진 겹침 현상으로 A박스의 위쪽 여백과 B박스의 아랫쪽 여백이 부모요소의 마진으로 표현되어 마치 자식요소의 마진값이 표현되지 않은 것 처럼 보인다.  
이걸 해결해 줄 수 있는게 `overflow: hidden;`속성이다.  

![overflow를 이용한 마진병합제거](https://images.velog.io/images/ursr0706/post/c112150b-9dad-42f1-946c-d9775e351810/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-19%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.26.50.png)

부모요소인 container에 `overflow: hidden;`속성이 적용되면서 새로운 BFC(Block Formatting Context)가 만들어졌기 때문에 자식요소의 마진이 모두 표현된 것이다.  
물론 부모요소에 부여했기 때문에 자식요소인 A와 B의 병합현상은 그대로 유지되면서 부모요소인 컨테이너와 박스A,B간의 병합현상만 해결할 수 있다.  
(만약 부모자식간, 형제간 모두 마진 병합 현상을 제거하려면 overflow를 이용하는 것 보다는 display속성의 값을 inline-block으로 입력하는게 좀 더 편하다.)

> overflow 참고사이트 - [MDN - overflow](https://developer.mozilla.org/ko/docs/Web/Guide/CSS/Block_formatting_context)  
> 참고영상 - [오버플로우 overflow | 코딩가나다 | 빔캠프](https://youtu.be/O-n1EjDEuIc)
