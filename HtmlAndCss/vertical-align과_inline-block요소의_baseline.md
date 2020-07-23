# vertical-align 속성과 Inline-block레벨 요소의 baseline

>vertical-align은 inline요소 또는 inline-block요소들끼리 수직 정렬을 어떻게 할지 지정하는 속성이다.  

(block레벨 요소에는 적용되지 않는 속성이다)

![vertical-align속성값 그림](https://images.velog.io/images/ursr0706/post/3703c8be-9181-4f27-bf5a-07e5251c67df/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.12.11.png)

- `baseline`: vertical-align의 기본값. 이미지와 텍스트 등의 요소는 기본적으로 이 baseline을 기준으로 배치가 된다. 글자가 앉아있는 선이다.  
한글과 다르게 영어소문자에만 있는 g, j, p, q, y와 같이 끝이 밑으로 내려가는 부분(decender)은 baseline 밑으로 내려간다.
- `top`: vertical-align상의 상단부분.
- `bottom`: vertical-align상의 하단부분.
- `middle`: vertical영역의 중간이 아닌 *알파벳 소문자의 중간*이 middle값이 된다. 폰트 종류가 바뀌게 되면 middle값도 상대적으로 바뀌게 된다.  
(만약 텍스트와 이미지를 나란히 놨을 때 이미지에 `vertical-align: middle`속성값을 넣으면 이미지는 텍스트의 소문자 가운데를 기준으로 수직 정렬이 되는데 이미지의 크기가 더 크다면 상관 없지만 텍스트가 이미지의 크기보다 큰 상황이라면 텍스트의 폰트에 따라 이미지의 위치가 달라진다.)  

Vertical-align속성값을 변경함에 따라 text의 수직정렬이 달라진다.  

## inline-block레벨 요소의 vertical-align

vertical-align속성은 inline-block레벨에서도 적용 가능한 속성이다.  
예시는 300px\*300px 의 크기를 가진 부모요소인 container에 100px\*100px의 크기를 갖은 자식요소인 box를 넣고 box요소에는 inline-block레벨로 display속성값을 적용하였다.  
vertical-align속성을 이용하여 세로 가운데에 위치하도록 시도하려 한다.  

```css
.container {
width: 300px;
height: 300px;
}

.box {
width: 100px;
height: 100px;
display: inline-block;
vertical-align: middle;
}
```

![test-vertical-align](https://images.velog.io/images/ursr0706/post/99525f5f-4212-41d6-9a68-ab801bf6e1f1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.28.22.png)

box에 `display:inline;`과 `vertical-align: middle;`속성값을 지정했지만 세로정렬이 가운데로 적용되지 않았다.  
왜 가운데로 오지 않는걸까?  
그 이유는 box가 활동할 수 있는 영역이 한정되어있기 때문이다.  

![vertical-align area](https://images.velog.io/images/ursr0706/post/8039d5b9-d920-49d3-98a0-72dfd09347bb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.24.34.png)

이렇게 box요소가 활동할 수 있는 영역은 빨간선의 한 줄로 한정되어있다.  
container의 세로크기가 아니라 그 한줄의 영역에서의 `vertical-align: middle;`이 적용된 것이다.  
세로 영역전체에서 가운데로 정렬하고 싶으면 한 줄의 높이를 container의 height크기만큼으로 늘려주면 된다.  

![edit_line-height](https://images.velog.io/images/ursr0706/post/e5a72138-e710-4bcd-b9b7-cf4a3d95571d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.44.16.png)

```css
.container {
line-height: 300px;
/* 한 줄의 높이를 300px로 적용 */
}
```

이렇게 부모요소에 height의 크기만큼 line-hiehgt값을 적용해주면 box의 세로정렬 활동영역이 container의 height만큼 늘어나면서 그에 대한`vertical-align: middle;`이 적용되어 container의 세로 가운데로 위치할 수 있는 것이다.
(line-height는 보통 text의 줄간격을 조정할 때 쓰는 속성인데, 위에서 살펴봤듯이 box요소가 vertical-align속성으로 활동할 수 있는 영역의 높이라고 할 수 있다.)  

## inline-block요소의 baseline

inline-block은 경우에 따라 baseline이 다르게 적용된다.  
바로 **text content의 존재 여부**에따라 baseline이 결정되는데 inline-block레벨인 요소의 baseline을 여러가지 경우로 살펴보자.

### 비어있는 inline-block 요소의 경우

먼저 아무런 text를 포함하지 않은 inline-block요소의 baseline은 기본적으로 **margin-box의 하단**이 baseline이 된다.

![empty_inline-block](https://images.velog.io/images/ursr0706/post/0f8037c2-6b89-4322-8911-4d779542224a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.39.24.png)

위 그림처럼 요소의 밑에 작은 틈이 생기는 이유는 앞서 살펴봤듯이 영문자에 있는 하행문자(decender)의 표현을 위한 baseline 밑의 공간 때문이다.
[이전 글 - inline/inline-block하단에 생기는 알 수 없는 갭 현상](https://github.com/nr-baek/TIL/blob/master/HtmlAndCss/image%EC%9A%94%EC%86%8C%EC%9D%98_%EA%B0%AD_%ED%98%84%EC%83%81.md)

![baseline at margin-box](https://images.velog.io/images/ursr0706/post/4adfb73d-3d28-4346-950c-ed294ce047ce/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.46.17.png)

여기에 두 번째 상자에 margin-bottom값으로 30px을 주면 위의 그림 처럼 baseline으로부터 30px 떨어져 있는 것을 볼 수 있다.  
이렇게 비어있는 inline-block요소는 margin-box의 하단이 baseline이 된다.

### 텍스트가 담긴 inline-block 요소의 경우

inline-block요소가 text content를 담고있다면 normal flow에서 마지막 content 요소가 baseline이 된다.  

![baseline_at_textcontent](https://images.velog.io/images/ursr0706/post/bd48d2f4-8aaf-4924-b066-a3d70b4af312/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.02.01.png)

요소 안에 text content가 있는 경우에는 text의 baseline이 기준이 된다.  
마지막 content요소가 baseline이 된다 하니 text content가 여러줄인 경우도 살펴보자.  

![baseline_at_multiline](https://images.velog.io/images/ursr0706/post/115b614a-5c0f-4c97-980e-d7ec217e6937/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.17.14.png)

여러줄의 text content가 담겨있을 때 마지막줄의 content요소가 baseline의 기준이 되는 것을 볼 수 있다.  

![baseline_at_overflow](https://images.velog.io/images/ursr0706/post/5e4d5ddc-39df-43f0-8d14-2fc9fd29a21d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-22%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.17.37.png)

위 처럼 text content가 부모영역에서 넘쳐나왔더라도 normal flow상의 마지막 content 요소가 baseline이 되기 때문에 마지막 text content요소가 baseline의 기준이 된다.  
단, overflow속성이 기본값인 `visible`여야한다.  
그렇다면 overflow속성의 값을 다르게 주면 어떤 결과가 나타날까?  

### overflow속성값이 visible이 아닌 inline-block 요소의 경우

text content가 있지만 **overflow 속성이 visible이 아닌 경우**, baseline은 비어있는 inline-block요소와 같이 margin-box의 하단이다.

![overflow_hidden_baseline](https://images.velog.io/images/ursr0706/post/00c27416-dfd6-4239-b24c-4e358f1a3f57/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.23.24.png)

overflow 속성값을 visible이 아닌 다른 값을 주면 margin-box의 하단이 baseline의 기준이 된다.  
overflow속성의 값을 scroll이나 auto로 적용해도 위와 같이 margin-box의 하단이 baseline의 기준이 된다.  
(margin-bottom에 값이 있다면 그 값만큼 떨어져보인다.)

![many_diffrent_baseline](https://images.velog.io/images/ursr0706/post/15333e0d-6e66-49eb-904d-2985b293204d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.16.12.png)

이렇게 inline-block레벨 요소에 text content가 담겨있는지,  
text content가 담겨있다면 overflow속성이 visible로 적용되었는지 아닌지의 여부에 따라 baseline의 기준이 달라진다.  

> 참고영상 - [버티컬얼라인 vertical-align | 코딩가나다 | 빔캠프](https://youtu.be/gkyxAkA-6oU)
